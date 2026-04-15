# Trading Bot - Project Instructions

## Application Purpose
This is an algorithmic trading bot for personal use.

## Strategy Implementation Rules
- Strategies live in the `strategies/` folder.
- When implementing a strategy, follow the specifications **precisely**.
- If any part of a strategy cannot be implemented exactly as specified due to API limitations or missing capabilities, **do not adjust or approximate the implementation** — stop and inform the user of the specific gap before proceeding.
- Every unsupported requirement must be explicitly reported before any workaround is considered.

## Brokerage Integration
- The primary brokerage API is **Alpaca** (currently on the **free plan**).
- Assume the account will eventually be upgraded to a paid plan when the bot matures.
- If a strategy requirement cannot be fulfilled due to a limitation of the **free Alpaca plan** (e.g., data access, order types, endpoints), stop and report the gap to the user rather than working around it.

## Trading Modes
- Every strategy must support both **paper trading** and **real (live) trading**.
- The mode should be configurable without changing strategy logic.

## Backtesting
- A backtesting mechanism must be maintained to test strategies and evaluate the impact of changes.
- Backtests should be runnable against historical data for any implemented strategy.

## Strategy Change Log
- Each strategy must have its own **change log** tracking all modifications over time.
- The change log must include: date of change, description of what changed, and the reason/motivation.
- This is required so strategy versions can be correlated with backtest results to understand which version performs better.
- Format: maintain a `CHANGELOG.md` (or equivalent) per strategy, co-located with or named after the strategy file.
