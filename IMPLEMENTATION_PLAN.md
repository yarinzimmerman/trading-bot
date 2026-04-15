# Plan: SPY 0DTE ITM Options Strategy Implementation

## Context

Implementing the full strategy defined in `strategies/SPY-0DTE-Strategy-Spec (1).md`. The project currently has zero Python code — this is a greenfield build. The strategy trades 0DTE SPY options based on intraday S/R level breaks with multi-chart confluence confirmation, chart-based stops, and a 3-phase trade management process.

---

## API Stack

| API | Role | Cost |
|-----|------|------|
| **Alpaca** (Algo Trader Plus) | Execution (options paper + live) + all market data (stocks, options chains, Greeks, pre-market, historical bars) | $99/month |
| **Schwab Trader API** (via `schwab-py`) | `$ADD` live streaming (NYSE Advance/Decline Issues) | Free — requires a Schwab brokerage account |
| **yfinance** | ES Futures (`ES=F`) historical data for backtesting only (optional, GAP-003) | Free |

**Total: $99/month for live trading. $0 during development and backtesting** (Alpaca free plan covers historical data; Schwab is always free; yfinance is free).

### Alpaca Plan Details
- **Free plan**: IEX exchange only for stocks; indicative (delayed/modified) options feed. Sufficient for backtesting and development.
- **Algo Trader Plus ($99/month)**: All US exchanges; full OPRA real-time options feed with Greeks (delta, gamma, theta, vega) + IV; unlimited API calls. Required for live/paper trading.
- Options **execution** is commission-free on both plans — the $99/month is for data only.

### Schwab Trader API Note
Uses OAuth 2.0 — requires a one-time browser login to obtain a token. `schwab-py` handles automatic token refresh after that. Not a simple API key setup, but manageable as a one-time configuration step.

---

## Resolved Gaps

All three original gaps are resolved with this stack. No workarounds, no approximations for live trading.

| ID | Gap | Resolution |
|----|-----|------------|
| GAP-001 | Options order execution | **Resolved** — Alpaca supports commission-free options execution (paper + live) on both free and paid plans |
| GAP-002 | `$ADD` NYSE Advance/Decline Issues (required for confluence check) | **Resolved** — Schwab Trader API streams `$ADVN`/`$DECN` in real time, from which the raw net A/D number is derived |
| GAP-003 | ES Futures data (optional) | **Resolved for backtesting** — yfinance `ES=F`; not needed for live/paper since this is optional |

---

## Directory Structure

```
trading-bot/
├── .env.example
├── requirements.txt
├── pyproject.toml
├── config/
│   ├── settings.py             # Pydantic BaseSettings — all mode/risk switches
│   └── constants.py            # Market hours, ET timezone, sizing tiers
├── core/
│   ├── broker/
│   │   ├── base.py             # Abstract BrokerAdapter
│   │   └── alpaca_broker.py    # Alpaca REST + WebSocket (live + paper endpoints)
│   ├── data/
│   │   ├── base.py             # Abstract DataProvider
│   │   ├── alpaca_data.py      # SPY/QQQ/AAPL bars, options chains + Greeks, pre-market
│   │   ├── schwab_data.py      # $ADD live streaming via schwab-py
│   │   ├── yfinance_data.py    # ES=F historical (backtesting only, optional)
│   │   └── aggregator.py       # Routes all data requests to correct provider
│   ├── options/
│   │   ├── selector.py         # ITM strike selection
│   │   └── pricing.py          # Black-Scholes approximation (backtesting only)
│   ├── risk/
│   │   └── position_sizer.py   # 10/20/30 contracts per tier
│   └── exceptions.py           # ConfluenceError, DataUnavailableError
├── strategies/
│   ├── base_strategy.py        # Abstract Strategy interface
│   └── spy-0dte-itm/
│       ├── CHANGELOG.md        # Created first — inception entry
│       ├── strategy.py         # Top-level orchestrator
│       ├── levels.py           # SRLevel, FibPivots, LevelSet data classes
│       ├── premarket_setup.py  # Part 1: all 6 setup steps
│       ├── confluence.py       # Part 2: 5-signal checker
│       ├── entry.py            # Part 3: Scenario A + Scenario B
│       ├── trade_manager.py    # Part 6: Phase 1/2/3 state machine + Orange Box
│       └── exits.py            # Part 7: stop, target, time, EOD exits
├── backtest/
│   ├── engine.py               # Day-by-day bar replay
│   ├── data_loader.py          # Historical bars (Alpaca + yfinance for ES=F)
│   ├── options_simulator.py    # Synthetic option P&L via Black-Scholes
│   ├── portfolio.py            # Tracks cash, positions, trades
│   ├── metrics.py              # Sharpe, drawdown, win rate, exit reason breakdown
│   └── report.py               # CSV/HTML report output
├── execution/
│   ├── order_manager.py        # Order lifecycle
│   ├── event_loop.py           # Main runtime loop with APScheduler
│   └── trade_journal.py        # trades.csv + signals.jsonl
├── scripts/
│   ├── run_live.py
│   ├── run_paper.py
│   └── run_backtest.py
└── tests/
    ├── unit/                   # No network; mock DataAggregator + BrokerAdapter
    └── integration/            # Real API calls; marked @pytest.mark.integration
```

---

## Key Design Decisions

### Paper vs. Live Toggle
Entirely `.env`-driven — zero code changes required:
```
TRADING_MODE=paper  →  ALPACA_BASE_URL=https://paper-api.alpaca.markets
TRADING_MODE=live   →  ALPACA_BASE_URL=https://api.alpaca.markets
```
`Settings` (Pydantic `BaseSettings`) loads once at startup. The broker factory reads `TRADING_MODE` and returns the appropriate `AlpacaBroker` instance. No `if mode == "live"` branches in strategy code.

### $ADD Data Flow
`core/data/schwab_data.py` wraps `schwab-py`'s streaming client. It subscribes to the `$ADVN` and `$DECN` NYSE streaming symbols and computes the raw net A/D value (`$ADVN - $DECN`) on each update. `aggregator.py` exposes this as a unified `get_add_value()` call used by `confluence.py`. For backtesting, `aggregator.py` routes to yfinance `^ADD` historical data instead.

### Options Data Flow (Live/Paper)
`core/data/alpaca_data.py` fetches the SPY option chain via Alpaca's `/v1beta1/options/snapshots` endpoint (OPRA feed on Algo Trader Plus), which returns real-time quotes + Greeks for all contracts. `core/options/selector.py` picks the first ITM strike from this chain. Real Greeks from Alpaca are used directly — Black-Scholes is only needed in backtesting where live options data is unavailable.

### Trade Manager State Machine
`trade_manager.py` uses a Python `Enum` for phases: `IDLE → PHASE_1 → PHASE_2 → PHASE_3 → CLOSED`, with `ORANGE_BOX` as a sub-state. On each bar close, `on_bar()` returns a `TradeAction` enum value that `event_loop.py` hands to `order_manager.py`.

### Backtest Options P&L
Historical 0DTE options prices are not available. `backtest/options_simulator.py` uses Black-Scholes with 30-day realized vol of SPY. Every backtest trade record includes `pricing_method="black_scholes_approx"`. This limitation is documented in `strategies/spy-0dte-itm/CHANGELOG.md`.

---

## Implementation Phases

### Phase 0 — Repository Bootstrap
- Clean git workspace check, create branch `feature/spy-0dte-itm`
- Create full directory skeleton + `__init__.py` files
- `pyproject.toml`, `requirements.txt`, `.env.example`
- `config/settings.py`, `config/constants.py`
- `core/exceptions.py`
- **Create `strategies/spy-0dte-itm/CHANGELOG.md`** — inception entry noting resolved gaps and backtest approximation caveat
- Gate: `pytest` collects 0 tests, 0 errors; Settings load from `.env.example`

### Phase 1 — Data Layer
- `core/data/base.py` — abstract interface
- `core/data/alpaca_data.py` — stock bars (1-min, 3-min, 5-min), pre-market bars, options snapshots + Greeks
- `core/data/schwab_data.py` — `$ADD` live streaming via `schwab-py` (`$ADVN`/`$DECN`)
- `core/data/yfinance_data.py` — `ES=F` historical for backtesting (optional)
- `core/data/aggregator.py` — routes live requests to Alpaca/Schwab; routes backtest requests to Alpaca historical + yfinance
- Integration tests: SPY 1-min bars, QQQ 3-min bars, options chain snapshot, `$ADD` stream (marked `@pytest.mark.integration`)
- Gate: all four data sources return valid data for a known past date/session

### Phase 2 — Pre-Market Setup (Part 1 of spec)
- `strategies/spy-0dte-itm/levels.py` — all data classes (`SRLevel`, `FibPivots`, `LevelSet`, `DayLevels`, `PremarketLevels`)
- `strategies/spy-0dte-itm/premarket_setup.py` — steps 1–6:
  - Step 1: yesterday's high/low from Alpaca daily bars
  - Step 2: pre-market high/low/midline from Alpaca pre-market bars (~8:00 AM snapshot)
  - Step 3: Fibonacci pivots `P=(H+L+C)/3`, R1-R3/S1-S3 with 0.382/0.618/1.0 multipliers
  - Step 4: 1-month S/R via 30-min bars — detect zones with 2+ candle body closes at same price (±tolerance)
  - Step 5: ES ATR ratio — optional; sourced from yfinance `ES=F` if enabled, otherwise skipped and levels marked `UNAVAILABLE`
  - Step 6: scan 9:00–9:30 bars for watch level
- Unit tests: Fibonacci math, 2-body S/R detection, duplicate level filtering
- Gate: `PremarketSetup.run()` produces a complete `LevelSet` for any historical day

### Phase 3 — Options Layer
- `core/options/selector.py`: `select_itm_strike(price, "call") → floor(price)`, `select_itm_strike(price, "put") → ceil(price)`
- `core/options/pricing.py`: Black-Scholes with realized vol estimation (backtesting only)
- `core/risk/position_sizer.py`: returns 10/20/30 based on `SIZING_TIER`
- Unit tests against known values
- Gate: `select_itm_strike(527.75, "call") == 527`; B-S prices a reasonable 0DTE ATM option

### Phase 4 — Strategy Logic (Parts 2–7 of spec)
- `confluence.py`: 5-signal checker (SPY 1min, QQQ 3min, $ADD, SPY 5min, AAPL 3min); full minimum of 3-of-3 required signals now achievable
- `entry.py`: Scenario A (9:30 open break), Scenario B (intraday break + retest within 0.05–0.10)
  - Short-term trend confirmation: HH+HL (calls) or LL+LH (puts)
  - Stop distance check: chart stop must be ≤20 cents from entry or signal is rejected
- `trade_manager.py` state machine:
  - Phase 1: fixed stop, monitor first S/R target
  - Phase 2: exit 1/3 at first target, move stop to broken S/R entry level, update hard stop (+30c)
  - Phase 3: trail stop below each candle wick after second S/R breaks
  - Orange Box detection and handling
  - Time stop: flat ≥`FLAT_STALE_MINUTES` or ≥15:30 ET
- `exits.py`: `EXIT_FULL`, `EXIT_PARTIAL`, trailing stop, EOD hard close, Orange Box break
- `strategy.py`: orchestrator wiring all modules
- Integration test: replay one known historical day end-to-end, verify signals/exits are deterministic
- Gate: full day simulation produces ≥1 entry signal, manages trade through phases, exits at EOD

### Phase 5 — Execution Layer
- `core/broker/alpaca_broker.py` — live and paper order submission via Alpaca REST
- `execution/order_manager.py`, `event_loop.py` (APScheduler), `trade_journal.py`
- Journal fields: date, entry/exit time, direction, strike, contracts, entry/exit option price, P&L, confluence score, exit reason, scenario (A/B), stop levels, phase reached
- Gate: paper mode runs one simulated session end-to-end, writes journal entries, exits cleanly at 15:30 ET

### Phase 6 — Backtesting Engine
- `backtest/data_loader.py` — historical bars from Alpaca; `^ADD` from yfinance; `ES=F` from yfinance (optional)
- `backtest/options_simulator.py` — Black-Scholes P&L approximation from SPY price path
- `backtest/portfolio.py`, `metrics.py`, `engine.py`, `report.py`
- Metrics: total P&L, win rate, Sharpe, max drawdown, exits by reason, confluence score distribution
- Gate: `scripts/run_backtest.py` produces a metrics report over a 30-day window with no unhandled exceptions

### Phase 7 — Entrypoints + Final Verification
- `scripts/run_live.py`, `run_paper.py`, `run_backtest.py`
- Final `CHANGELOG.md` entry with implementation completion notes and backtest approximation caveats
- Gate: all three scripts start cleanly; journal has entries; CHANGELOG is current

---

## Dependencies

```
alpaca-py>=0.13.0           # Alpaca REST + WebSocket (execution + data)
schwab-py>=1.0.0            # Schwab Trader API — $ADD streaming
yfinance>=0.2.40            # ES=F historical (backtesting, optional)
pandas>=2.1.0
numpy>=1.26.0
scipy>=1.12.0               # Black-Scholes N(d1)/N(d2) for backtesting
pydantic>=2.5.0
pydantic-settings>=2.1.0
apscheduler>=3.10.4         # Pre-market 8:00 AM ET cron
pytz>=2024.1
pytest>=8.0.0
pytest-asyncio>=0.23.0
pytest-mock>=3.12.0
freezegun>=1.4.0            # Freeze time in market-hours unit tests
```

---

## Verification

After each phase gate and at final completion:
1. Run `pytest tests/unit/` — must pass with no failures
2. Run `scripts/run_paper.py` for one simulated day — verify `journal/trades.csv` has a new row and `journal/signals.jsonl` has confluence check entries with all 5 signals populated
3. Verify `$ADD` values in `journal/signals.jsonl` are sourced from Schwab (not yfinance) in live/paper mode
4. Run `scripts/run_backtest.py` over 5-day window — verify `BacktestResult` includes metrics and `pricing_method="black_scholes_approx"` on all trades
5. Verify `strategies/spy-0dte-itm/CHANGELOG.md` is current
