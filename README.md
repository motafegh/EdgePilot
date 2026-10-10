# EdgePilot

EdgePilot is an evidence-driven cryptocurrency trading research and execution project.

The project starts from a simple premise: **capital should not be exposed because a strategy sounds plausible; a strategy must earn the right to manage capital through reproducible evidence, explicit risk controls, and staged validation.**

## Current objective

Build a personal trading laboratory that can progress deliberately through:

1. market and trading fundamentals,
2. historical-data research,
3. reproducible strategy evaluation,
4. live paper trading,
5. tightly bounded real-money experiments,
6. evidence-based scaling,
7. and only later, where justified, automation.

The initial capital discussed for eventual live experimentation is small ($100–$200). This is **not** a daily-profit target or a promise of returns. The first objective is preservation of capital and trustworthy learning.

## Core principles

- **Evidence before deployment.** Backtests, simulations, and live observations must support a strategy before capital is exposed.
- **Capital preservation before profit maximization.** Risk limits are first-class system constraints.
- **No forced daily target.** Performance is judged statistically over meaningful samples, not by requiring profit every day.
- **Separate research from execution.** Hypotheses, evidence, decisions, and orders should remain distinguishable.
- **Reproducibility.** Important results should be reconstructable from retained data, configuration, code revision, and assumptions.
- **Uncertainty is valid output.** The system may conclude that evidence is insufficient and decline a trade.
- **Human control first.** Initial live operation remains human-approved. Automation is earned rather than assumed.
- **Complexity must justify itself.** ML, LLMs, agents, order-book models, and additional strategies are research options, not default requirements.

## Repository map

The foundational documentation lives under `docs/`:

- `docs/PROJECT_CHARTER.md` — mission, scope, success criteria, and non-goals.
- `docs/RISK_POLICY.md` — capital-protection and deployment gates.
- `docs/ARCHITECTURE.md` — initial conceptual architecture and responsibility boundaries.
- `docs/ROADMAP.md` — staged path from research to bounded live deployment.
- `docs/decisions/` — durable architectural and product decisions.
- `docs/working-memory/` — current state, open questions, and immediate next work.

Repository operating rules for AI-assisted work are recorded in `AGENTS.md`.

## Status

**Bootstrap / project-definition stage.** No trading strategy, exchange, market universe, execution technology, or implementation stack has yet been selected.

## Important

EdgePilot is a research and software-engineering project, not financial advice and not a guarantee of profit. Cryptocurrency markets can produce rapid and substantial losses.