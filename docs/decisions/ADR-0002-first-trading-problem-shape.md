# ADR-0002: First trading problem shape — low-turnover spot swing research

- **Status:** Accepted
- **Date:** 2026-10-10

## Context

EdgePilot needs a first trading problem that is compatible with approximately $100–$200 of eventual designated capital, the initial spot-only risk boundary, reproducible research, and staged deployment.

The candidate space includes different operating horizons (scalping, intraday, swing), different edge hypotheses (trend following, mean reversion), and structurally different approaches (arbitrage, market making).

The project must narrow this space before building strategy-specific infrastructure, but it must not mistake problem selection for evidence of profitability.

The supporting comparison is recorded in:

`docs/research/2026-10-10_PHASE_A_TRADING_STYLE_SELECTION.md`

## Decision

EdgePilot will use the following as its **first research problem shape**:

> **Low-turnover spot swing trading on highly liquid crypto markets, with trend following as the primary first strategy family to investigate and mean reversion retained as the first major comparator.**

For the initial project stages:

- **selected operating horizon:** swing / hours-to-days rather than seconds-to-minutes,
- **selected market type:** spot,
- **primary first strategy family:** trend following / momentum,
- **retained comparator:** mean reversion,
- **not selected as first targets:** scalping, high-turnover intraday trading, cross-venue arbitrage, and market making.

This decision does **not** establish that trend following is profitable and does not authorize real-money trading.

## Rationale

The selected problem shape best matches the current constraints because it:

- reduces sensitivity to fees, spread and execution latency relative to very short-horizon strategies,
- avoids requiring low-latency/order-book infrastructure before its value is established,
- can be researched with obtainable spot-market historical data,
- supports explicit and reproducible rules,
- permits clear position-risk and invalidation reasoning,
- does not require constant manual monitoring,
- and provides a direct path from research to live paper operation and later bounded execution.

Trend following is selected as the first hypothesis family because simple variants are falsifiable, interpretable and useful for learning market regime, volatility, risk, whipsaw and execution-cost concepts. Mean reversion remains a meaningful contrasting hypothesis rather than being silently discarded.

## Rejected first targets

### Scalping

Rejected for the first target because small gross price moves are highly sensitive to fees/spread/slippage and execution quality. It also introduces latency and microstructure complexity too early.

### General high-turnover intraday trading

Deferred because turnover and monitoring burden are higher without providing enough benefit for the first research system.

### Cross-venue arbitrage

Deferred because capital fragmentation, synchronized execution, venue differences, transfer/settlement constraints and executable-price evidence become first-order concerns.

### Market making

Deferred because queue position, adverse selection, continuous quoting, inventory control and exchange microstructure would dominate the first implementation.

## Explicitly unresolved

ADR-0002 does not choose:

- the target exchange,
- the exact asset universe,
- quote currency,
- candle interval/timeframe,
- exact trend definition,
- entry/exit rules,
- stop mechanism,
- numerical risk-per-trade limit,
- data provider,
- backtesting framework,
- or execution implementation.

These remain Phase A decisions to earn from additional mechanics, cost, availability and data evidence.

## Consequences

### Positive

- Phase B infrastructure can optimize for trustworthy low-frequency spot research instead of premature high-frequency complexity.
- Backtesting can begin with ordinary market data rather than order-book reconstruction.
- Early experiments can focus on research validity rather than latency engineering.
- The project retains a contrasting strategy family for later comparison.

### Costs

- Fewer trades mean statistical evidence may accumulate more slowly in live operation.
- Swing positions remain exposed to abrupt market movement between evaluations.
- Trend-following strategies may suffer repeated small losses in sideways/choppy regimes.
- The selected direction may later be rejected by strategy evidence; this ADR only chooses where to investigate first.

## Revisit conditions

Revisit this decision if evidence shows that:

- costs or market constraints make the selected horizon impractical,
- available data cannot support trustworthy research,
- another strategy class provides a clearly superior first-learning/research tradeoff,
- or later infrastructure reduces execution complexity enough to justify a different horizon.

A short winning or losing streak is not sufficient reason to change this ADR.
