# Phase A Research: First Trading Problem Selection

**Date:** 2026-10-10  
**Status:** Completed comparison; supports ADR-0002

## Question

Which trading problem is the best first target for EdgePilot given:

- approximately $100–$200 of eventual designated capital,
- capital preservation over forced daily profit,
- spot-only initial scope,
- a desire for a measurable and reproducible system,
- staged progression from research to paper to bounded live use,
- and the need to learn market mechanics without immediately depending on fragile execution infrastructure?

## First clarification: these categories are not all the same kind of thing

Trading terminology often mixes several dimensions.

### Operating horizon

- **Scalping:** seconds to minutes; many small moves.
- **Intraday/day trading:** positions generally opened and closed within a day.
- **Swing trading:** positions usually last from hours to multiple days or longer.

### Edge hypothesis

- **Trend following / momentum:** expects persistence in directional movement.
- **Mean reversion:** expects sufficiently stretched movement to move back toward some reference or equilibrium.
- Other hypotheses may later exist, but they are not required for the first target.

### Market-structure strategies

- **Arbitrage:** attempts to exploit price differences across instruments/venues or related structures.
- **Market making:** continuously quotes liquidity and attempts to earn spread/rebates while controlling inventory and adverse-selection risk.

Therefore EdgePilot does not need to choose "swing versus trend following". A coherent first target can be **spot swing trading using a trend-following hypothesis**.

## Why small capital changes the optimization problem

At $100–$200, the first objective should not be maximizing trade count. The account is large enough to place ordinary spot orders on many major markets, but small enough that:

1. fixed/percentage trading costs can consume a meaningful fraction of small expected moves,
2. splitting capital across many simultaneous positions quickly creates tiny notional sizes,
3. high turnover creates many opportunities for fees, spread, slippage and implementation mistakes,
4. high-frequency execution does not gain us much absolute profit even if the gross percentage edge is positive,
5. the primary value of early live capital is evidence about execution and behavior, not income.

### Current fee evidence

Exchange fees vary materially, which is itself important.

As checked on 2026-10-10:

- Binance's published regular-user spot schedule lists 0.10% maker / 0.10% taker for its general spot schedule.
- Kraken's current lowest spot tier lists 0.40% maker / 0.80% taker.
- Coinbase Advanced states that maker/taker fees depend on fee tier and rolling volume and can change; the current rate should be checked in the account/order preview.

These examples are **not an exchange recommendation**. They demonstrate that execution venue can change the economics of the same strategy substantially.

A $50 position paying 0.10% on entry and 0.10% on exit costs about $0.10 before spread/slippage. At 0.80% taker each way it costs about $0.80. A strategy trying to capture a 0.5% move on $50 has only $0.25 gross movement before costs. Therefore very small target moves can be structurally unattractive for a low-volume account depending on venue and execution style.

Relevant official sources:

- https://www.binance.com/en/fee/trading
- https://www.kraken.com/features/fee-schedule
- https://help.coinbase.com/en/coinbase/trading-and-funding/advanced-trade/advanced-trade-fees

Order minimums also vary by pair and venue. They are unlikely to make a $100–$200 spot experiment impossible on major pairs, but they matter for position sizing and portfolio fragmentation and must later be obtained programmatically or from the target venue's current rules.

## Comparison criteria

The first EdgePilot target should score well on:

1. **small-capital compatibility** — sensible positions without leverage or excessive fragmentation,
2. **cost tolerance** — edge is not dominated by fees/spread/slippage,
3. **execution tolerance** — does not require sub-second precision or sophisticated order-book placement,
4. **data simplicity** — can be researched reliably using obtainable historical market data,
5. **statistical testability** — rules and outcomes can be reproduced and measured,
6. **operational burden** — does not require constant human monitoring,
7. **risk clarity** — entry, invalidation, size and maximum planned loss are understandable,
8. **automation path** — can later be automated without changing the underlying strategy idea,
9. **educational value** — teaches mechanics that remain useful for later strategy families.

Scores below are comparative judgments for **EdgePilot's first project target**, not universal statements about profitability.

| Candidate | Small capital | Cost tolerance | Execution tolerance | Research simplicity | Monitoring burden | First-target fit |
|---|---:|---:|---:|---:|---:|---:|
| Swing + trend following | High | High | High | High | Low | **Very high** |
| Swing + mean reversion | High | High | Medium–High | High | Low | **High** |
| Intraday trend/momentum | Medium | Medium | Medium | Medium–High | Medium–High | Medium |
| Intraday mean reversion | Medium | Medium | Medium | Medium | Medium–High | Medium |
| Scalping | Low | Low | Low | Medium | Very high | **Low** |
| Cross-venue arbitrage | Low–Medium | Medium | Low | Medium | High | **Low** |
| Market making | Low | Low–Medium | Very low | Low–Medium | Very high | **Very low** |

## Candidate analysis

### 1. Scalping

**What it attempts:** capture many very small short-duration price movements.

**Why it is unattractive as the first target:**

- expected move per trade is small, so fees/spread/slippage consume a larger share of gross edge,
- latency and fill quality matter much more,
- order-book behavior becomes important earlier,
- a large number of trades magnifies implementation and operational errors,
- it creates a strong temptation to optimize noisy short-horizon behavior,
- with $100–$200, even successful small-percentage moves generate very small absolute profits.

**Decision:** reject as the first EdgePilot problem. It may be researched later if the project develops execution infrastructure and evidence that justifies it.

### 2. General intraday trading

**What it attempts:** complete positions within a day, using momentum, breakout, reversion or other hypotheses.

**Advantages:**

- many observations can accumulate faster,
- overnight exposure can be reduced,
- strategies can be objectively specified.

**Costs for us:**

- higher turnover than swing approaches,
- greater monitoring and execution sensitivity,
- more temptation to confuse activity with evidence,
- costs remain more important because targets are generally smaller and trades more frequent.

**Decision:** keep as a future candidate, not the first research target.

### 3. Swing trading

**What it means here:** signals and positions operating on a horizon of hours to days rather than seconds/minutes, without a requirement to close every day.

**Why it fits EdgePilot:**

- lower turnover reduces the dominance of trading costs,
- execution does not initially require low-latency infrastructure,
- major spot markets provide ample observable price/volume history,
- entries, invalidation and position sizing can be reasoned about clearly,
- live paper operation can run continuously without requiring the human to stare at a screen,
- it creates a clean path from historical research to live signal generation and later controlled execution.

**Decision:** select **swing / low-frequency spot operation** as the first operating horizon.

### 4. Trend following / momentum

**Core hypothesis:** once sufficiently defined directional movement exists, some degree of continuation may be exploitable before the trend ends.

**Why it is a strong first hypothesis family:**

- rules can be simple and explicit,
- it does not require estimating an exact fair value,
- price-based inputs make data provenance relatively straightforward,
- failure modes are understandable: whipsaw, sideways regimes, delayed entries, abrupt reversals,
- it naturally teaches regime, volatility, stop distance, risk/reward and position sizing.

The project must not assume trend following is profitable. It is merely a strong **first research family** because it is falsifiable and implementable.

**Decision:** select trend following as the primary first strategy family to investigate.

### 5. Mean reversion

**Core hypothesis:** sufficiently unusual displacement from a reference condition is followed by partial or full reversion.

**Why it remains valuable:**

- explicit rules are possible,
- it contrasts meaningfully with trend following,
- its failure cases are educational, especially when a supposed temporary deviation is actually the beginning of a new trend,
- it can later act as a comparator or regime-specific alternative.

**Decision:** retain as the **first major comparator**, but do not develop two production strategies in parallel during initial infrastructure work.

### 6. Arbitrage

**Potential attraction:** the apparent edge can look more directly economic than directional prediction.

**Why it is not the first target:**

- cross-venue strategies often require capital pre-positioned on multiple venues,
- execution sequencing and latency become part of the edge,
- fees, withdrawal/transfer constraints, settlement delays and venue-specific risks matter,
- opportunities visible in historical snapshots may not have been simultaneously executable,
- small capital becomes fragmented.

**Decision:** reject as the first target; revisit only with a specific arbitrage mechanism and executable evidence.

### 7. Market making

**Potential attraction:** attempts to earn spread rather than predict large directional moves.

**Why it is especially poor for our first target:**

- requires continuous quoting/cancel/reprice behavior,
- fill probability and queue position matter,
- adverse selection can dominate nominal spread capture,
- inventory must be actively controlled,
- exchange microstructure, latency and fee/rebate schedules are central,
- professional competition is strong.

**Decision:** out of scope for the initial project stages.

## Selected first problem shape

EdgePilot should initially target:

> **Low-turnover spot swing trading on highly liquid crypto markets, with trend following as the first strategy family to research and mean reversion retained as a deliberate comparator.**

This does **not** yet select:

- a specific exchange,
- BTC, ETH or another exact asset universe,
- a candle interval,
- a specific trend indicator,
- entry/exit thresholds,
- stop-loss implementation,
- exact risk percentage,
- or a backtesting framework.

Those decisions require their own evidence.

## Why this is better than immediately choosing a popular strategy

The decision constrains the engineering problem without pretending the edge is already known.

It tells us what infrastructure matters first:

- reliable spot OHLCV/trade data,
- fees and order constraints,
- deterministic signal evaluation on hour/day-scale data,
- position/risk modeling,
- reproducible backtests,
- and later live data/paper execution.

It also tells us what **does not** need to exist yet:

- sub-second market-data feeds,
- colocated execution,
- complex order-book simulators,
- multi-venue balance management,
- derivatives liquidation modeling,
- or autonomous high-frequency execution.

## What we should learn next

The next Phase A responsibility is to understand the mechanics necessary to specify the first research problem correctly:

1. spot pair/base/quote semantics,
2. market vs limit vs stop-style orders,
3. maker/taker behavior,
4. bid/ask spread and slippage,
5. OHLCV candle semantics and timeframe aggregation,
6. liquidity and volume,
7. return, volatility and drawdown,
8. risk per trade and position sizing,
9. expectancy and risk/reward,
10. why fees and execution costs must be modeled in backtests.

After those mechanics are understood, we can earn the next decisions: initial market universe, exchange/data constraints, timeframe range and numerical risk policy.

## Validity boundary

This selection is about **project suitability**, not a claim that swing trend following has positive expected value. Profitability remains completely unestablished until a specific hypothesis is researched and validated.
