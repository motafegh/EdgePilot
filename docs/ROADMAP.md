# EdgePilot Roadmap

The roadmap is staged by **evidence maturity**, not by a fixed calendar. Later phases begin only when the earlier responsibility is sufficiently understood and verified.

## Phase 0 — Bootstrap and governance

**Purpose:** establish project boundaries before implementation creates accidental commitments.

Current work:

- project charter,
- risk/deployment policy,
- conceptual architecture,
- governance and working memory,
- initial accepted decision record.

**Exit:** enough shared context exists to make the first domain decisions without guessing what the project is trying to become.

## Phase A — Trading mechanics and problem selection

**Purpose:** choose a realistic first trading problem while learning the mechanics needed to reason about it.

Topics include:

- spot-market mechanics,
- order types and fills,
- spread, liquidity, volatility, fees and slippage,
- candles and market data semantics,
- position sizing and risk/reward,
- expectancy and drawdown,
- comparison of candidate styles such as swing, intraday, trend-following, and mean reversion.

Decisions to earn:

- first market universe,
- first time horizon/trading style,
- exchange/data constraints,
- initial numerical risk limits.

**Exit:** one constrained research target and explicit reasons for selecting it.

## Phase B — Research infrastructure

**Purpose:** make experiments trustworthy before trying to discover an edge.

Expected capabilities:

- acquire and validate historical data,
- retain provenance and versioned configuration,
- model fees and basic execution costs,
- run reproducible backtests/experiments,
- compute performance and risk metrics,
- retain experiment records,
- detect obvious leakage/look-ahead mistakes.

**Exit:** a deliberately simple control strategy can be reconstructed and evaluated end to end even if it is unprofitable.

## Phase C — First strategy research

**Purpose:** test one understandable trading hypothesis rather than accumulate indicators.

Work includes:

- define hypothesis and causal/behavioral rationale,
- specify exact rules,
- establish baselines,
- evaluate robustness and sensitivity,
- separate development and validation evidence,
- test relevant market regimes/failure cases,
- reject the strategy if evidence is insufficient.

**Exit:** either a justified rejection or a strategy that satisfies the research and validation gates. A rejected strategy is a successful research outcome if the conclusion is trustworthy.

## Phase D — Live paper operation

**Purpose:** expose the system to real market timing and operational conditions without capital risk.

Evaluate:

- live data reliability,
- signal timing,
- operational failures,
- realistic spread/liquidity behavior,
- divergence from backtest assumptions,
- journal completeness,
- and human usability of proposals.

**Exit:** behavior is sufficiently understood to decide whether a bounded live-money experiment is justified.

## Phase E — Bounded live experiment

**Purpose:** test the smallest sensible real-money deployment under explicit risk limits.

Initial properties:

- spot only,
- small subset of designated project capital,
- precomputed downside,
- human approval,
- complete decision/execution journal,
- explicit pause/stop conditions.

**Exit:** enough observations exist to compare actual costs, execution, behavior, and drawdown against expectations.

## Phase F — Evidence-based scaling

Possible work:

- revise position sizing from observed risk,
- define portfolio exposure limits,
- improve monitoring and failure handling,
- increase capital gradually if justified,
- distinguish strategy deterioration from normal variance.

Scaling is optional. If evidence does not support it, exposure stays unchanged or is reduced.

## Phase G — Advanced research and automation

Only after demonstrated need, investigate capabilities such as:

- multiple strategies,
- regime-specific strategy selection,
- portfolio allocation,
- ML,
- order-book or on-chain features,
- sentiment/news,
- LLM-assisted investigation,
- multi-agent research,
- autonomous execution.

No item in this phase is required for EdgePilot to be successful.

## Cross-phase rule

Each phase should produce four things:

1. **understanding** — what we learned,
2. **artifact** — code/data/docs needed to retain it,
3. **verification** — evidence that the responsibility works as claimed,
4. **decision** — what this evidence permits us to do next.
