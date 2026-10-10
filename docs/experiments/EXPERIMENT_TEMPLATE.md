# Experiment: <short descriptive name>

- **Status:** Planned | Running | Completed | Rejected | Superseded
- **Date:** YYYY-MM-DD
- **Code revision:** <commit SHA>
- **Owner:** <name/agent>

## Question

What exact question is this experiment trying to answer?

## Hypothesis

State the testable claim before inspecting the result.

## Why this matters

What project decision could this evidence affect?

## Inputs and provenance

Record enough information to reproduce the experiment:

- market/instrument(s),
- data source and retrieval/version details,
- date/time window,
- timeframe/bar construction,
- preprocessing/features,
- strategy/configuration version,
- fees/spread/slippage assumptions,
- random seeds where relevant,
- relevant environment/tool versions.

## Method

Describe the procedure, baselines, train/development/validation separation, and metrics.

## Acceptance / rejection criteria

Define what evidence would support, weaken, or fail to establish the hypothesis before evaluating the result where practical.

## Results

Record observations and quantitative outputs without interpretation first.

## Interpretation

Explain what the observations support and what they do not establish.

## Risks and validity threats

Examples:

- look-ahead bias,
- survivorship bias,
- overfitting/data snooping,
- too-small sample,
- unrealistic fills/costs,
- regime concentration,
- missing/corrupt data,
- implementation errors.

## Decision

What, if anything, may the project do differently because of this experiment?

Use **no decision / insufficient evidence** when appropriate.

## Follow-up

List only the next questions directly exposed by this experiment.
