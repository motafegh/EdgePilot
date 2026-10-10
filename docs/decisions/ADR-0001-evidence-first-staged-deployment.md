# ADR-0001: Evidence-first, staged deployment with human control

- **Status:** Accepted
- **Date:** 2026-10-10

## Context

EdgePilot begins with approximately $100–$200 of potential future trading capital. A tempting objective would be to optimize immediately for a fixed daily return, but doing so can create pressure to overtrade, increase risk after losses, or interpret short-term luck as evidence of skill.

The project also needs to remain useful if the first strategy fails, because learning whether an apparent edge is real is itself part of the product.

## Decision

EdgePilot will use an **evidence-first staged deployment model**:

1. learn the relevant market mechanics,
2. build reproducible research capability,
3. test explicit hypotheses,
4. validate candidate strategies,
5. operate them in live paper conditions,
6. begin any real-money experiment with bounded exposure and human approval,
7. scale only when observed evidence justifies it.

A fixed daily-profit requirement is rejected as a governing objective.

The initial live scope excludes leverage, margin, derivatives, and autonomous execution. These are not permanently forbidden, but adding them requires explicit future decisions and stronger risk evidence.

## Consequences

### Positive

- Capital preservation is not subordinated to an arbitrary daily target.
- Failed hypotheses can be rejected without being treated as project failure.
- Backtest evidence and live evidence remain distinct.
- Automation can be added after its operational and risk requirements are understood.
- The project can learn from small capital without requiring high percentage returns to appear successful.

### Costs

- The project will reach real-money automation more slowly.
- Some attractive-looking strategies will be rejected or delayed.
- More work is required to retain evidence, configuration, costs, and decision history.

These costs are accepted because the alternative would make it difficult to distinguish a real edge from overfitting, execution mistakes, or luck.

## Revisit conditions

Revisit this ADR only if accumulated evidence demonstrates that a different deployment model provides a superior safety/research tradeoff. A desire for faster profit is not sufficient by itself.
