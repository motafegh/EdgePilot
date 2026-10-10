# EdgePilot Risk and Deployment Policy

**Status:** Initial policy. Numerical thresholds not yet accepted are explicitly marked **Proposed**.

This document governs when EdgePilot may progress from research toward real-money exposure. Its purpose is not to eliminate trading risk; that is impossible. Its purpose is to prevent avoidable, unbounded, or poorly understood risk from entering the project.

## Accepted initial boundaries

The following are part of the project's current direction:

- Research and paper/live simulation come before real-money automation.
- Capital preservation has higher priority than meeting a profit target.
- There is no requirement to make money every day.
- Human approval is required during the initial live-money stage.
- Initial scope excludes leverage, margin, derivatives, borrowing, and liquidation-risk positions.
- Loss-recovery schemes that increase risk because previous trades lost (for example martingale-style sizing) are out of scope.
- A strategy may remain inactive for long periods. No-trade is a valid outcome.
- A strategy must be evaluated after trading costs, not only before fees/spread/slippage.
- Live exposure must begin below the total capital available for the project.

## Deployment gates

### Gate 0 — Research integrity

Before paper trading, a strategy must have:

- explicit entry, exit, and invalidation rules,
- deterministic or versioned configuration,
- reproducible historical evaluation,
- realistic cost assumptions,
- recorded sample size and evaluation window,
- relevant risk metrics,
- evidence that obvious data leakage/look-ahead errors have been checked,
- and documented limitations/unknowns.

### Gate 1 — Validation integrity

Before live paper operation, the strategy must demonstrate that the apparent edge is not merely the result of fitting the same data used to invent it. The exact validation technique will depend on strategy type, but some separation of development evidence from validation evidence is required.

### Gate 2 — Live paper behavior

Before real-money deployment, EdgePilot must run against live market conditions without capital exposure long enough to evaluate:

- signal timing,
- data freshness and failures,
- spread and liquidity behavior,
- expected versus observable execution assumptions,
- missed/duplicate signals,
- operational stability,
- and divergence from historical expectations.

Passing a backtest alone cannot satisfy this gate.

### Gate 3 — Bounded real-money experiment

The first real-money stage must:

- use only a bounded fraction of designated project capital,
- use spot trading only under the initial scope,
- define maximum loss per position before entry,
- define stop conditions for the experiment,
- preserve complete execution and decision records,
- and require human approval of each trade.

### Gate 4 — Scaling

Capital exposure may increase only after enough real-money observations exist to compare actual behavior with research assumptions. Scaling should be based on measured risk-adjusted behavior and operational reliability, not on a short winning streak.

## Proposed numerical starting constraints

These values are **candidates for later acceptance, not current hard requirements**:

- risk per trade: around **0.5% of total designated project capital**,
- initial real-money allocation: materially below the full $100–$200 available,
- explicit daily/weekly drawdown stop,
- explicit maximum simultaneous exposure,
- minimum sample requirements before scaling.

We should set these only after deciding the strategy style, expected trade frequency, fees, exchange constraints, and market universe. A numerical limit without that context can create false precision.

## Strategy failure and pause conditions

A live strategy should be pausable when any of the following occurs:

- behavior materially diverges from the validated assumptions,
- data or execution integrity becomes uncertain,
- configured risk limits cannot be enforced,
- actual costs materially exceed evaluated assumptions,
- a code/configuration revision invalidates prior deployment evidence,
- the strategy enters a market condition for which its behavior is not established,
- or retained records are insufficient to audit what happened.

Pausing is not strategy failure. Continuing without trustworthy evidence is a process failure.

## Security risk is trading risk

Compromised exchange credentials can dominate every statistical trading risk. Exchange integration must therefore follow `SECURITY.md` and use least privilege, with withdrawal capability disabled by default.

## Review rule

Any later decision to introduce leverage, derivatives, autonomous live execution, materially larger capital, or loss-dependent position sizing requires an explicit new risk decision rather than a silent extension of this policy.
