# EdgePilot Project Charter

## Mission

Build an evidence-driven cryptocurrency trading research and execution system that helps a small account learn safely, test whether repeatable trading edges exist, and expose capital only when the evidence and risk controls justify it.

## Starting context

- Intended eventual starting capital: approximately **$100–$200**.
- The project does **not** assume a fixed daily-profit target.
- Early success means learning, reproducibility, and capital preservation before it means return maximization.
- The first live stage should be intentionally smaller than the total available capital.

## Primary goals

1. Learn the market mechanics required to reason correctly about trading decisions.
2. Build trustworthy historical and live market-data workflows.
3. Form explicit, testable trading hypotheses.
4. Evaluate strategies with realistic fees, spread/slippage assumptions, and out-of-sample thinking.
5. Separate evidence, interpretation, risk assessment, decision, and execution.
6. Maintain a durable journal of experiments and eventually trades.
7. Progress from research to paper trading and only then to bounded live deployment.
8. Scale exposure only when measured evidence justifies it.

## Non-goals for the initial project

- Guaranteed or fixed daily income.
- Turning a small account into a large account quickly.
- High leverage or liquidation-risk strategies.
- Maximizing trade frequency.
- Copying indicator recipes without validating them.
- Treating an LLM as an oracle for price direction.
- Building a large multi-agent or ML platform before a simpler system proves insufficient.
- Supporting many exchanges, assets, and strategies from the beginning.

## Product concept

The initial system should evolve toward a personal **trading laboratory**:

`Market data -> candidate opportunity -> evidence -> interpretation -> risk assessment -> trade proposal -> human decision -> execution/simulation -> outcome -> post-trade evaluation`

Later automation may compress parts of this chain, but responsibility boundaries and traceability should remain visible.

## What constitutes an edge?

For EdgePilot, an edge is not a pattern that looks convincing on a chart. It is a decision rule or model that, under clearly stated assumptions, demonstrates positive expected value or another explicitly justified objective across sufficiently robust evaluation while remaining compatible with the project's risk constraints.

Any claimed edge must state at minimum:

- market/instrument universe,
- timeframe and holding assumptions,
- entry and exit logic,
- position-sizing logic,
- data source and evaluation window,
- fees and execution assumptions,
- sample size,
- relevant risk metrics,
- validation method,
- known failure conditions and uncertainty.

## Initial success criteria

Before live capital is considered, the project should be able to:

- reconstruct an experiment reproducibly,
- explain why a candidate trade exists,
- calculate bounded downside before execution,
- model realistic trading costs,
- distinguish in-sample from validation evidence,
- preserve no-trade outcomes,
- and evaluate strategy behavior across different conditions rather than only aggregate profit.

The live phase will require additional gates in `RISK_POLICY.md`.

## Decision philosophy

A strategy does not receive capital because it is interesting. It must **earn deployment**.

A deployed strategy does not receive more capital because it recently won. It must **earn scaling**.

A complex technique does not enter the architecture because it is sophisticated. It must **earn complexity** by improving a defined weakness under controlled evaluation.
