# EdgePilot Current State

**Last updated:** 2026-10-10

## Where we are

EdgePilot has been created and is in **Phase 0 — Bootstrap and governance**.

The project direction is established, but no strategy, exchange, market universe, data source, backtesting framework, implementation stack, or live-trading mechanism has been selected yet.

## Accepted direction

- EdgePilot is an evidence-driven crypto trading research and execution project.
- The eventual starting capital under discussion is approximately $100–$200.
- A fixed `$1/day` or similar daily-profit target is **not** a system requirement.
- Capital preservation and trustworthy learning come before profit maximization.
- Research -> validation -> live paper operation -> bounded live experiment -> evidence-based scaling is the controlling deployment sequence.
- Initial live operation remains human-approved.
- Initial scope excludes leverage, margin, derivatives, borrowing, and liquidation-risk positions.
- An LLM may later assist research, interpretation, review, or operation, but it is not assumed to be the primary price-direction oracle.
- A strategy must earn deployment; complexity must earn inclusion; capital increases must earn scaling.

## Foundational artifacts

- `README.md`
- `AGENTS.md`
- `docs/PROJECT_CHARTER.md`
- `docs/RISK_POLICY.md`
- `docs/ARCHITECTURE.md`
- `docs/ROADMAP.md`
- `docs/decisions/ADR-0001-evidence-first-staged-deployment.md`

## Explicitly unresolved

These decisions should not be guessed or silently chosen:

1. first trading style/time horizon,
2. first market and asset universe,
3. target exchange and legal/operational constraints,
4. historical and live data sources,
5. exact numerical risk limits,
6. first strategy hypothesis,
7. implementation/backtesting stack,
8. persistence and experiment-record format,
9. live-paper execution method,
10. eventual exchange integration design,
11. reporting/UI needs,
12. tax/accounting record requirements relevant to actual operation.

## Next coherent responsibility

Begin **Phase A — Trading mechanics and problem selection**.

The immediate question is not "which indicator should we code?" It is:

> Which trading style is realistically compatible with our small starting capital, intended risk level, available attention, trading costs, and desire to build a measurable, learnable system?

The first comparison should cover at least:

- swing trading,
- intraday trading,
- scalping,
- trend following,
- mean reversion,
- and, for contrast, approaches such as arbitrage/market making that may be structurally unsuitable at this account size.

The result should narrow the problem space and identify what market mechanics we need to learn next.

## Stop line

Do not begin implementing a production trading strategy or connecting exchange trading credentials before the Phase A problem-selection work establishes why that implementation is the right first target.
