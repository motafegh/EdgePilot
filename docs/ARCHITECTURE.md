# EdgePilot Initial Architecture

This is a conceptual architecture, not an implementation commitment. It exists to keep responsibilities separate while we learn what the system actually needs.

## Core flow

```text
Market/Data Sources
        |
        v
Acquisition + Normalization
        |
        v
Research / Feature Construction
        |
        v
Strategy Hypothesis + Candidate Generation
        |
        v
Evidence Package
        |
        +--> Risk Evaluation
        |
        +--> Contradiction / Uncertainty Checks
        |
        v
Trade Proposal
        |
        v
Human Decision  (initial live mode)
        |
        v
Simulation or Execution
        |
        v
Outcome + Journal
        |
        v
Post-trade / Strategy Evaluation
```

## Responsibility boundaries

### 1. Market data

Responsible for retrieving and retaining the observations needed for research and live decisions. It should eventually make data source, timestamp, symbol/instrument identity, interval, and freshness explicit.

It does not decide whether to trade.

### 2. Research layer

Responsible for exploration, hypothesis formation, feature construction, backtesting, validation, and statistical evaluation.

Research output is evidence about a defined experiment, not a live order.

### 3. Strategy layer

Responsible for expressing a testable decision rule or model and producing candidate opportunities under known configuration.

A strategy candidate is not automatically permission to execute.

### 4. Evidence package

A candidate trade should carry the information needed to evaluate it rather than only a `BUY`/`SELL` label. Candidate evidence may eventually include:

- strategy/configuration identity,
- current market/regime observations,
- triggering conditions,
- relevant contradicting signals,
- expected entry/exit/invalidation conditions,
- estimated costs,
- and provenance back to the observations used.

### 5. Risk engine

Responsible for deciding whether a proposed exposure fits project constraints and for calculating bounded position size/downside.

Risk policy must be capable of rejecting a statistically attractive trade.

### 6. Decision layer

Initially human-owned for real-money trading. It consumes the proposal, supporting evidence, risk evaluation, and known uncertainty.

A later automated decision owner would require an explicit architectural and risk-policy decision.

### 7. Execution layer

Responsible only for translating an approved decision into exchange/simulation operations and reporting what actually happened: requested order, accepted order, fills, fees, partial fills, failures, cancellations, and timing.

Requested execution and observed execution must remain distinguishable.

### 8. Journal and evaluation

Responsible for retaining enough context to reconstruct why a trade was proposed, what decision was made, what execution occurred, and what outcome followed.

The journal feeds strategy evaluation but should not rewrite historical facts when interpretations later change.

## Important separations

EdgePilot should not collapse these pairs:

- market observation vs interpretation,
- hypothesis vs validated result,
- backtest result vs live evidence,
- strategy signal vs risk approval,
- approval vs requested order,
- requested order vs actual fill,
- profit/loss vs evidence of a repeatable edge,
- historical validity vs current validity after material change.

## Candidate future capabilities — not initial commitments

These may become useful later if evidence justifies them:

- multiple concurrent strategies,
- portfolio/risk allocation,
- market-regime models,
- order-book features,
- on-chain data,
- news/sentiment inputs,
- ML models,
- LLM-assisted research and review,
- multi-agent research workflows,
- autonomous execution,
- multiple exchanges or asset classes.

Each should enter through a defined problem and evaluation, not through architectural speculation.

## Implementation stance

We intentionally have not yet selected:

- programming/runtime stack beyond likely Python-oriented research,
- exchange,
- data vendor,
- storage system,
- backtesting framework,
- market universe,
- time horizon,
- deployment environment,
- or UI.

Those choices should follow the first domain and operational decisions rather than precede them.
