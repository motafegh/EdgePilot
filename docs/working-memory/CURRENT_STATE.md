# EdgePilot Current State

**Last updated:** 2026-10-10

## Where we are

EdgePilot has completed its bootstrap/governance baseline and has advanced into **Phase A — Trading mechanics and problem selection**.

The core first-problem selection is now established. Phase A is **substantially complete but not closed** because exchange/jurisdiction constraints and final numerical live-risk limits remain intentionally unresolved.

## Accepted project direction

- EdgePilot is an evidence-driven crypto trading research and execution project.
- The eventual starting capital under discussion is approximately $100–$200.
- A fixed `$1/day` or similar daily-profit target is **not** a system requirement.
- Capital preservation and trustworthy learning come before profit maximization.
- Research -> validation -> live paper operation -> bounded live experiment -> evidence-based scaling is the controlling deployment sequence.
- Initial live operation remains human-approved.
- Initial scope excludes leverage, margin, derivatives, borrowing, and liquidation-risk positions.
- An LLM may later assist research, interpretation, review, or operation, but it is not assumed to be the primary price-direction oracle.
- A strategy must earn deployment; complexity must earn inclusion; capital increases must earn scaling.

## Accepted Phase A decisions

### First trading problem shape

ADR-0002 selects:

> **Low-turnover spot swing trading on highly liquid crypto markets, with trend following as the primary first strategy family to investigate and mean reversion retained as the first major comparator.**

Not selected as initial targets:

- scalping,
- high-turnover intraday trading,
- cross-venue arbitrage,
- market making.

This is a research-direction decision, not evidence that trend following is profitable.

### Initial research universe

ADR-0003 selects:

- **BTC**
- **ETH**

The exact quote currency/pair remains dependent on venue/funding constraints.

### Initial timeframe

- **4H** is the primary candidate decision timeframe for the first strategy research.
- **1D** may be retained for explicit broader-regime/context or robustness work.
- Minute/second-scale decision-making is out of the first target.

## Phase A mechanics learned/recorded

`docs/learning/PHASE_A_MARKET_MECHANICS_PRIMER.md` now establishes the working concepts needed before backtesting:

- base/quote and spot semantics,
- bid/ask/spread,
- market, limit and stop-style orders,
- maker/taker,
- slippage and liquidity,
- OHLCV semantics and intrabar ambiguity,
- timeframe meaning,
- return and volatility,
- risk per trade and position sizing,
- reward/risk and expectancy,
- drawdown,
- cost-adjusted trade accounting,
- key backtest failure modes.

## Exchange/data position

The project has defined mandatory exchange/data requirements in:

`docs/research/2026-10-10_PHASE_A_EXCHANGE_AND_DATA_REQUIREMENTS.md`

A provider must satisfy legal/account availability, liquid BTC/ETH spot markets, machine-readable trading rules, historical/live data, trading APIs, least-privilege credentials, cost observability and small-account compatibility.

No exchange is selected yet because legal/operational availability depends on the user's actual jurisdiction/account-access situation and must not be inferred.

The preferred architecture is provider-neutral core records behind exchange/data adapters.

## Current artifacts

### Governance / project

- `README.md`
- `AGENTS.md`
- `SECURITY.md`
- `docs/PROJECT_CHARTER.md`
- `docs/RISK_POLICY.md`
- `docs/ARCHITECTURE.md`
- `docs/ROADMAP.md`

### Decisions

- `docs/decisions/ADR-0001-evidence-first-staged-deployment.md`
- `docs/decisions/ADR-0002-first-trading-problem-shape.md`
- `docs/decisions/ADR-0003-initial-research-universe-and-timeframe.md`

### Phase A research / learning

- `docs/research/2026-10-10_PHASE_A_TRADING_STYLE_SELECTION.md`
- `docs/research/2026-10-10_PHASE_A_EXCHANGE_AND_DATA_REQUIREMENTS.md`
- `docs/learning/PHASE_A_MARKET_MECHANICS_PRIMER.md`

### Experiment support

- `docs/experiments/EXPERIMENT_TEMPLATE.md`

## Explicitly unresolved

The following should not be silently guessed:

1. target exchange and actual legal/account availability,
2. exact quote currency/pair for BTC and ETH,
3. historical/live data source implementation,
4. final numerical live-risk limits,
5. exact first trend-following hypothesis and rules,
6. implementation/backtesting stack,
7. persistence and experiment-record format,
8. live-paper execution method,
9. eventual exchange integration design,
10. reporting/UI needs,
11. tax/accounting record requirements relevant to actual operation.

## Next coherent responsibility

Finish the remaining Phase A decisions, then enter Phase B.

### Remaining Phase A

1. establish actual jurisdiction/account-access constraints for exchange selection,
2. compare the viable venues against the recorded rubric,
3. choose quote markets/data source,
4. convert the proposed live risk limits into accepted numerical policy if the venue/minimum-order economics support them.

### Then Phase B

Design and implement the **research foundation**, not a production trading bot:

- canonical market-data records,
- provider adapter boundary,
- historical acquisition and validation,
- reproducible datasets/configuration,
- realistic fee/cost model,
- deterministic backtest engine or selected framework,
- risk/performance metrics,
- experiment persistence,
- leakage/look-ahead safeguards,
- and a deliberately simple control strategy proving the research pipeline end to end.

## Stop line

Do not implement a production strategy, connect live trading credentials, or claim an edge before the Phase B research system can reproduce and audit its evidence.
