# SPY 0DTE ITM Options Trading Strategy — Full Spec

> Based on: *"How I Draw My Lines"*, *"A+ Setup & Double Cross Indicators"*, *"Entries Exits & Stops"*, *"Support & Resistance — In Depth"*
> Underlying: **SPY (ETF)** | Timeframe: **1-Minute chart only** | Options: **0DTE, First ITM Strike**

---

## Philosophy & Foundation

Before any setup, chart, or trade — three core principles govern every decision:

| Principle | Definition |
|-----------|------------|
| **Probability** | A number reflecting the chance a particular event will occur. Every decision is about stacking probabilities, not certainty. |
| **Patience** | The ability to accept delay without becoming reactive. Waiting for the setup IS the strategy. |
| **Discipline** | Orderly, prescribed conduct and self-control. Without it, even a correct strategy produces inconsistent results. |

**The 3 C's of Discipline:**
- **Clarity** — Know exactly what your do's and don'ts are. No gray area.
- **Consistency** — Repeat good habits. Consistency builds confidence.
- **Consequences** — Violate clarity and consistency and you will lose money. Getting lucky without clarity creates a false sense of skill — you won't know why it worked or why it stops working.

> *"It is impossible to take a trade without the risk of potentially losing money. If you take a trade with the fear of being wrong, you're already on the wrong foot."*

---

## Line Color Reference

Every line on your chart has a specific color tied to its source. This is how you instantly know what a level represents at a glance.

| Color | Style | Source |
|-------|-------|--------|
| **Dark Blue** | Solid | Pre-market support / resistance / midline |
| **Pink** | Dotted | Yesterday's high and low |
| **Orange** (turns **Yellow** when touched) | Solid | Pivot points and 1-month S/R targets |
| **Teal** | Dotted | Futures ATR levels (converted to SPY price) |

---

## Part 1 — Pre-Market Chart Setup (Done Every Morning ~8:00 AM)

Perform this setup each morning around **8:00 AM ET**, before market open. By 8 AM, enough pre-market price action and volume exists to draw meaningful levels. The closer to open you finalize these lines, the more accurate they will be. All lines are plotted on a **1-minute SPY chart**.

---

### Step 1: Yesterday's High and Low
**Color: Pink Dotted — Source: Previous Day**

- Obtain yesterday's trading session high and low from any data source or broker platform.
- Plot both as **pink dotted horizontal lines** on the 1-minute SPY chart.
- Note whether today's open will gap above, below, or within yesterday's range — this context informs the day's directional bias.
- These levels act as price magnets. SPY frequently revisits them during the session.

---

### Step 2: Pre-Market Support, Resistance & Midline
**Color: Dark Blue Solid — Source: Pre-Market Price Action**

- Using the current pre-market session (reviewed around 8:00 AM ET), identify:
  - The **pre-market high**: the price level where the majority of candle bodies and wicks clustered at the top — not necessarily the single extreme spike.
  - The **pre-market low**: same logic at the bottom of the range.
- Plot both as **dark blue solid horizontal lines.**
- **Midline (optional third blue line):** Only add a midline if the pre-market range is wide enough to justify a third level. On tight days (roughly 40 cents or less), skip it. If yesterday's high or low falls naturally between the pre-market high and low, it can serve as the midline.
- These lines are valid reference levels during the trading day but carry less weight than intraday confirmed S/R.

---

### Step 3: Fibonacci Pivot Points
**Color: Orange Solid (turns Yellow once touched) — Source: Fibonacci Pivot Calculation**

Fibonacci pivot points are calculated from the **previous day's high, low, and closing price.** They can be calculated manually, via a spreadsheet, or pulled from any platform or data source that provides Fibonacci pivot calculations.

**Standard Fibonacci Pivot Formulas:**
```
Pivot (P)  = (High + Low + Close) / 3
R1 = P + 0.382 × (High - Low)
R2 = P + 0.618 × (High - Low)
R3 = P + 1.000 × (High - Low)
S1 = P - 0.382 × (High - Low)
S2 = P - 0.618 × (High - Low)
S3 = P - 1.000 × (High - Low)
```

**How to use them:**
- Identify the **2 closest attainable pivot levels below** current price (S1, S2) and the **1–2 closest above** (R1, R2). Attainable means realistically reachable within a single trading session.
- Plot each as a **solid orange horizontal line** and set a price alert on each.
- **Skip a pivot** if it falls within a few cents of a blue or pink line already on the chart — no duplicate lines.
- Fibonacci pivots only produce **3 levels per direction** (R1–R3 above, S1–S3 below). There is no R4 or S4. When you need targets beyond these, proceed to Step 4.
- Once price touches a pivot line during the day, mark it **yellow** as confirmation it acted as a level.

---

### Step 4: Additional S/R Targets Beyond Pivot Range
**Color: Orange Solid (turns Yellow once touched) — Source: 1-Month Historical Price Action**

When the Fibonacci pivots don't extend far enough (you need a level beyond R3 or S3, or the gap between two pivots is large), find additional levels from recent price history using a **30-minute chart zoomed out to approximately 1 month.**

**Rules for identifying valid levels:**
- **Single candles do not qualify.** You need at minimum **two candle bodies closed** at or near the same price to count it as a valid S/R zone. Wicks can supplement, but bodies confirm.
- **To find targets above R3:** Find the most recent period where price traded above R3. Identify the level where price double-topped, found strong resistance, or had multiple candle bodies close at the same area. That is your next target high. Continue scrolling back in time to find each successive level.
- **To find targets below S3:** Same process in reverse. Find the most recent period where price traded below S3 and locate the support that held. Keep going back in time for additional levels as needed.
- **Use the most recent information first.** A level that acted as support but was later broken has likely flipped to resistance. Do not reuse it as support without fresh confirmation.
- Plot these as **orange solid lines** — same as pivot lines. They turn yellow when touched.

---

### Step 5: Futures ATR Levels Converted to SPY
**Color: Teal Dotted — Source: ES Futures ATR Levels**

ES Futures (S&P 500 futures) trade at a different price scale than SPY. This step converts key futures-based support/resistance levels into their SPY equivalent so they can be plotted on the SPY chart.

**What levels to use:** Identify the **4 closest ATR-based support/resistance levels** on the ES Futures chart to the current futures price. These can come from any ATR level tool or indicator available on your platform.

**Calculate the futures-to-SPY conversion ratio (once per day):**
1. Open both an ES Futures chart and a SPY chart on the **same timeframe (1-minute).**
2. Pick **any 1-minute candle after 8:00 AM ET** — the specific candle does not matter. It simply must be the **exact same candle on both charts** (matched by timestamp).
3. Record the **high of that candle on futures** and the **high of that same candle on SPY.**
4. Calculate: `Ratio = Futures High ÷ SPY High`
   - Example: `4139.75 ÷ 411.52 = 10.0596`
5. This ratio is valid for the **entire trading day.** Calculate it once.

**Convert each futures level to SPY:**
```
SPY Equivalent = Futures Level ÷ Ratio
Example: 4150.25 ÷ 10.0596 = 412.57 → plot teal dotted line at 412.57
```

**Rules:**
- Plot each converted level as a **teal dotted horizontal line.**
- **Do not add a teal line if a line already exists at that price.** If the converted level lands on or very near an existing orange, blue, or pink line — skip it. No duplicates.
- Futures lines that fall on levels already broken during pre-market carry less weight during the trading day.

---

### Step 6: Scan the 9:00–9:30 Window on the 1-Minute Chart
**Source: Final 30 Minutes of Pre-Market**

After all lines are plotted, look at the **last 30 minutes of pre-market (9:00–9:30 AM ET)** on the 1-minute chart and identify whether an obvious support or resistance formed during that window — a level where price clearly bounced, rejected, or consolidated. Note this level as an **open watch level.**

This level is important because:
- If price breaks it cleanly on the **first candle at market open (9:30)**, that break alone is sufficient to consider a trade — no waiting period required.
- The break candle close is your signal. Confluence check (Part 2) must still be confirmed before entering.

Example: Between 9:00–9:30 SPY consolidates between 527.20 and 527.80. At 9:30 the first candle closes at 528.05 — a clean break above 527.80. That is a valid open trigger.

---

### Pre-Market Setup Checklist

- [ ] Yesterday's high and low calculated and plotted *(Pink Dotted)*
- [ ] Pre-market high and low identified at ~8:00 AM and plotted *(Dark Blue Solid)*
- [ ] Pre-market midline added only if range is wide enough to justify it *(Dark Blue Solid)*
- [ ] Fibonacci pivots calculated and plotted — no duplicates *(Orange Solid)*
- [ ] 1-month chart reviewed — additional targets beyond pivot range plotted, 2+ body closes required *(Orange Solid)*
- [ ] Futures-to-SPY ratio calculated using matching post-8AM candle
- [ ] 4 nearest futures ATR levels converted and plotted — no duplicates *(Teal Dotted)*
- [ ] 9:00–9:30 window scanned — open watch level noted if one exists
- [ ] Price alerts set on all plotted lines
- [ ] All lines saved / grouped by date for future reference

---

## Part 2 — Multi-Chart Confluence Check

Before entering any trade, verify that the broad market is moving in the same direction as the intended trade. This is done by checking the following charts simultaneously at the moment of entry.

### Minimum Required (must have all 3 to enter)

| Chart | Timeframe | What to Look For |
|-------|-----------|-----------------|
| SPY | 1-minute | Current candle trending in trade direction (green = calls, red = puts) |
| QQQ | 3-minute | Same direction as trade |
| $ADD | 1-minute | Rising and positive direction (calls) / falling and negative direction (puts) |

### Stronger Signal (bonus — not required but increases confidence)

| Chart | Timeframe | What to Look For |
|-------|-----------|-----------------|
| SPY | 5-minute | Same direction as trade |
| AAPL | 3-minute | Same direction as trade |

**$ADD — important platform note:**
- Ticker: **$ADD** (NYSE Advance/Decline issues)
- This is the **raw net number** of NYSE advancing stocks minus declining stocks — not the cumulative A/D line.
- For calls: $ADD should be rising and in positive territory, indicating the broad market is broadly participating upward.
- For puts: $ADD should be falling and in negative territory.
- Confirm your platform is displaying the raw net issues number. On most platforms the correct ticker is `$ADD` or `NYAD`.

### Probability Weighting

The more charts aligned, the higher the probability of a successful trade:

| Aligned | Confidence |
|---------|------------|
| SPY + QQQ + $ADD | Minimum — proceed with normal confidence |
| + SPY 5-min | Good |
| + AAPL 3-min | Strong |
| All 5 aligned | Highest conviction |

---

## Part 3 — Entry Rules

### Two Valid Entry Scenarios

#### Scenario A — Open Break (9:30 AM, no waiting period)
If a clear support or resistance was identified in the 9:00–9:30 window (Step 6 of pre-market setup) and the **first candle at market open closes through that level** — that break is a valid entry trigger. No waiting until 9:40 required. Confirm confluence (Part 2) and proceed to option selection.

#### Scenario B — Intraday Break and Retest (any time after open)
At any point during the session, a 1-minute candle closes through a known S/R level (orange/yellow, teal dotted, dark blue, or pink). Wait for the retest as described below.

---

### Step 1: Confirm a Short-Term Trend (Scenario B only)

Before entering an intraday trade, confirm a trend has established on the 1-minute chart:
- **Bullish:** Price makes a higher high and a higher low.
- **Bearish:** Price makes a lower low and a lower high.

This typically develops within the first 10 minutes of the session. Do not enter against an ambiguous or unconfirmed trend.

---

### Step 2: Identify the Break

A 1-minute candle **closes through a known S/R level** with conviction:
- For calls: candle closes **above a resistance level.**
- For puts: candle closes **below a support level.**

---

### Step 3: The Retest (with wiggle room)

**If a retest comes:**
The next candle pulls back toward the broken level. It does not need to touch the exact price — a return to **within 0.05–0.10 of the level** is sufficient. Example: level was 527.67, retest candle dips to 527.72 (5 cents above) and closes green — that qualifies. Enter at the close of that candle.

> *"When you move up and break through resistance and the retest candle stays above that line and stays green — momentum is still in your favor. If it immediately turns red, the sellers are still there."*

**If no retest comes:**
Price breaks the level and keeps running without pulling back. In this case, stronger confluence is required:
- SPY 1-min, QQQ 3-min, and $ADD must all be clearly aligned.
- Ideally SPY 5-min and AAPL 3-min are also aligned.
- If confluence is strong, enter at the close of the break candle or the following candle.
- If confluence is mixed or weak, skip the trade entirely and wait for the next setup.

---

### Step 4: Run the Confluence Check

At the close of the entry candle, verify Part 2 conditions. Minimum: SPY 1-min + QQQ 3-min + $ADD aligned. More aligned = higher confidence.

---

### Step 5: Confirm Stop Location on the Chart

Before entering, locate the stop loss level on the chart. The stop is always chart-based — identified by the structure, not calculated from a formula:
- **For calls:** Just below the wicks of the most recent candles that confirmed support at the entry S/R level.
- **For puts:** Just above the wicks of the most recent candles that confirmed resistance at the entry S/R level.

If the chart-based stop results in a distance of more than approximately 20 cents on SPY from the entry price, the entry is not tight enough — do not take the trade. A good entry naturally produces a tight stop because the structure is clean.

---

### What NOT to Trade

- Do not enter if there is an opposing S/R level immediately in the path of the trade — no room to reach the first target.
- Do not enter based on candle color or confluence alone without a confirming S/R level. Every trade must be justified by a line.
- Do not enter if the retest candle closes on the wrong side of the level.

---

## Part 4 — Option Selection

### Instrument

| Parameter | Specification |
|-----------|--------------|
| Underlying | SPY (ETF) |
| Expiration | **0DTE** — same-day expiration only |
| Option Type | Calls for bullish setups / Puts for bearish setups |
| Strike | **First ITM strike** |

**First ITM Strike:**
- **Calls:** First whole-dollar strike **below** current SPY price at entry. Example: SPY at $527.75 → buy the **$527 Call.**
- **Puts:** First whole-dollar strike **above** current SPY price at entry. Example: SPY at $527.75 → buy the **$528 Put.**

### Why First ITM?

- Delta ~0.80–0.85: moves nearly dollar-for-dollar with SPY
- Lower time value than ATM options
- Typical premium at open: **$1.50–$3.00 per contract** depending on volatility

### Position Sizing

| Tier | Contracts |
|------|-----------|
| Conservative | 10 contracts |
| Standard | 20 contracts |
| Aggressive | 30 contracts |

Size is selected before entry and does not change mid-trade.

### Entry Timing

- Enter at the **close of the confirmed entry candle** — not mid-candle.
- Preferred window: **9:30 AM – 1:00 PM ET.**
- After 2:00 PM ET, theta decay accelerates sharply on 0DTE. Only the cleanest, tightest setups warrant entry.
- **Do not hold 0DTE options past 3:30 PM ET** unless deep ITM and the move is actively in progress.

---

## Part 5 — Stop Loss Rules

### Primary Stop (Chart-Based, Candle Close)

The primary stop is always defined by chart structure — the price level at which the trade thesis is structurally invalidated:
- **For calls:** Just below the wicks of the candles that confirmed support at the entry level.
- **For puts:** Just above the wicks of the candles that confirmed resistance at the entry level.

This stop is triggered on a **candle close** beyond the level — not on a wick pierce. A candle that closes back on the correct side of the level means the trade is still valid.

> *"A wick will come down, stop you out, and we've all had to go back in the direction we wanted. If it wicks down but closes back above — you're still in the trade."*

### Hard Stop (Safety Net — Mechanical)

Because a fast violent move can push price well past the stop level before a candle closes, a **hard mechanical stop** is also set as a safety net:

- Place the hard stop **30 cents below the chart-based stop level** on SPY (for calls), or 30 cents above (for puts).
- This stop triggers immediately if price reaches it — regardless of whether the candle has closed.
- It is not a normal exit — it is a "don't get destroyed" floor for runaway moves only.
- **Adjust the hard stop every time the chart-based stop moves** to maintain the 30-cent gap.

### Thesis Validity

> **As long as the S/R level holds, the trade thesis remains valid — regardless of how much the option premium has moved.**

Risk is defined by chart structure, not premium bleed. If the level is holding, hold the position.

---

## Part 6 — Trade Management

### Phase 1 — Entry to First Target: Fixed Stop

From entry until the first S/R target is reached, **do not move the stop.** Keep it exactly where it was placed at entry — just beyond the retest wicks. Let the trade develop without premature tightening. The hard mechanical stop (30 cents below) remains in place throughout.

### Phase 2 — First Target Hit: Partial Exit + Stop to Broken S/R

When price reaches or comes within **0.05–0.10** of the first S/R target:

1. **Exit one-third of the position** at or near the target level.
2. **Move the stop** (both chart-based and hard stop) to the **S/R level that was broken to enter the trade** — not to break-even, but to the actual plotted line that was broken and retested on entry. This level is a meaningful chart structure point, cleaner than an arbitrary entry price.
3. Adjust the hard stop to sit 30 cents beyond this new stop level.

At this point worst case: small profit on 1/3, approximately break-even on the remaining 2/3 (since the stop is now at the entry S/R level). You cannot be seriously hurt by this trade anymore.

### Phase 3 — Next S/R Level Breaks: Trail the Remaining Two-Thirds

Once price pushes through the first target level and breaks a **second S/R level:**

1. Switch to **trailing the stop below each candle's wick** as candles close (for calls) or above each candle's wick (for puts).
2. After each new confirming candle close, move the stop to just beyond that candle's wick.
3. Continue until the trailing stop is triggered.
4. The hard mechanical stop adjusts alongside the trailing stop, always maintaining the 30-cent gap.

> You will stop out slightly below the peak — this is correct and expected. You are capturing most of the extended move.

### The Orange Box — Consolidation Zones

If price enters a sideways consolidation during an active trade:
1. Draw a box around the consolidation range.
2. Hold as long as price stays inside the box.
3. Break in the direction of the original move → hold, target the next level, begin trailing once through.
4. Close out of the box against your position → exit the trade.

### Spotting Weakness

Signs to watch for that suggest momentum is fading:
- First opposing candle after a sustained run in trade direction
- Wicks getting progressively longer at new highs/lows
- Price stalling at or approaching a known strong S/R level
- $ADD reversing direction against the trade
- QQQ or SPY 5-min candle flipping color against the trade

---

## Part 7 — Exit Scenarios

There are two ways a trade ends:

### Exit 1 — Stop Hit Before First Target

A candle closes beyond the chart-based stop level (or the hard mechanical stop triggers first on a fast move). Exit the **full position** immediately at market. No exceptions on 0DTE — do not wait, do not hope for recovery.

### Exit 2 — First Target Hit, Then Trail

1. **First target reached:** Exit 1/3 of the position. Move stop to the broken S/R entry level. Hard stop adjusts to 30 cents beyond.
2. **Second S/R level breaks:** Begin trailing the stop below each candle's wick on the remaining 2/3.
3. **Trailing stop triggered:** Exit the remaining 2/3 at market. Trade is complete.

**Additional time-based exit:**
If the trade is flat or minimally profitable after 45–60 minutes with no directional movement, consider exiting. Stagnant 0DTE positions bleed premium. After 3:30 PM ET, exit all 0DTE positions regardless of status unless deep ITM and actively moving in your favor.

---

## Part 8 — Full Trade Checklist

### Pre-Market (~8:00 AM ET)
- [ ] Yesterday's high and low plotted *(Pink Dotted)*
- [ ] Pre-market high and low plotted *(Dark Blue Solid)*
- [ ] Pre-market midline added if range warrants it *(Dark Blue Solid)*
- [ ] Fibonacci pivots calculated and plotted — no duplicates *(Orange Solid)*
- [ ] 1-month chart reviewed — additional targets beyond pivot range, 2+ body closes *(Orange Solid)*
- [ ] Futures-to-SPY ratio calculated
- [ ] 4 futures ATR levels converted and plotted — no duplicates *(Teal Dotted)*
- [ ] 9:00–9:30 window scanned — open watch level noted *(if exists)*
- [ ] Price alerts set on all lines
- [ ] Lines saved / grouped by date

### Trade Entry Gate (All Must Pass)
- [ ] Valid entry scenario: open break (9:30) OR intraday break + retest
- [ ] For intraday: short-term trend confirmed (higher highs/lows or lower lows/highs)
- [ ] Clean break of a known S/R level *(Orange/Yellow, Teal Dotted, Dark Blue, or Pink)*
- [ ] Entry candle closed on correct side of level, correct color
- [ ] Retest within 0.05–0.10 of level (if retest came); or strong 5-of-5 confluence (if no retest)
- [ ] Confluence check: SPY 1-min ✓ QQQ 3-min ✓ $ADD direction ✓ (minimum 3)
- [ ] Chart-based stop identified — approximately 20 cents or less from entry
- [ ] No opposing S/R immediately blocking path to first target

### Option Selection
- [ ] Expiration = today (0DTE)
- [ ] Strike = first ITM (first strike below price for calls / first strike above for puts)
- [ ] Contract count selected: 10 / 20 / 30
- [ ] Entry at candle close

### Trade Active
- [ ] Chart-based stop set just beyond retest wicks
- [ ] Hard mechanical stop set 30 cents beyond chart stop
- [ ] Phase 1: stop fixed — do not move until first target hit
- [ ] Phase 2 (first target): 1/3 exited, stop moved to broken S/R entry level, hard stop adjusted
- [ ] Phase 3 (second S/R breaks): trailing stop below each candle wick, hard stop adjusted
- [ ] Weakness signals monitored ($ADD direction, QQQ color, wick length)

### Exit
- [ ] Full exit if stop hit before first target
- [ ] 1/3 exit at first target → stop to broken S/R → trail remaining 2/3
- [ ] Remaining 2/3 exit when trailing stop triggered
- [ ] Time stop: exit if flat 45–60 min, or all positions closed by 3:30 PM ET

---

## Summary — The Strategy in One Paragraph

*Every morning around 8:00 AM ET, plot yesterday's high and low (pink dotted), pre-market high and low (dark blue solid), Fibonacci pivot points (orange solid), additional 1-month S/R targets (orange solid), and futures ATR levels converted to SPY price (teal dotted). Scan the 9:00–9:30 pre-market window for an obvious level that could break at open. At 9:30, if that level breaks cleanly, enter immediately after the confluence check — SPY 1-min, QQQ 3-min, and $ADD all moving in the same direction. For intraday trades, confirm a short-term trend, wait for a break and retest (within 0.05–0.10 of the level), and run the same confluence check. Locate a chart-based stop just beyond the retest wicks — if it's more than ~20 cents away, skip the trade. Buy the first ITM 0DTE call or put (10–30 contracts) at candle close. Set a hard mechanical stop 30 cents beyond the chart stop as a safety net. Hold with the fixed stop until the first S/R target — take 1/3 off, move the stop to the broken S/R entry level. Once price breaks the next S/R level, trail the stop below each candle wick on the remaining 2/3 until stopped out. Every trade must be justified by a line.*

---

*Strategy source: "How I Draw My Lines", "A+ Setup & Double Cross Indicators", "Entries Exits & Stops", "Support & Resistance — In Depth"*
