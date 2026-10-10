# ADR-0003: Initial research universe and timeframe

- **Status:** Accepted
- **Date:** 2026-10-10

## Context

ADR-0002 selected low-turnover spot swing trading as EdgePilot's first problem shape, with trend following as the first hypothesis family. Phase B now needs a sufficiently constrained universe and time horizon so that the research infrastructure is built for a concrete problem rather than an unlimited crypto market.

Adding many assets at the beginning would increase:

- data-quality surface area,
- multiple-testing opportunities,
- asset-selection and survivorship bias,
- venue-specific market-rule complexity,
- and the temptation to search across coins until a profitable-looking backtest appears.

Using only one asset, however, would make it easier to mistake asset-specific historical behavior for a more general strategy property.

Current 2026 liquidity research continues to show BTC as the deepest major crypto market and ETH as another highly liquid major market across leading centralized venues.

## Decision

### Initial asset universe

The first EdgePilot research universe will be:

- **Bitcoin (BTC)**
- **Ethereum (ETH)**

against the selected venue's appropriate liquid quote market.

The exact quote asset/currency remains unresolved until exchange, jurisdiction, funding and data constraints are selected.

No other crypto asset enters the initial strategy-development universe unless a later explicit decision expands it.

### Initial research timeframe

The first strategy research will use **4-hour candles as the primary candidate decision timeframe**.

Daily (`1D`) data may be retained and used for:

- broader market/regime context,
- robustness comparisons,
- or deliberately specified higher-timeframe features.

The initial strategy must not silently mix 4H and 1D information. Any cross-timeframe use must be explicit and correctly aligned to information that would have been available at decision time.

Lower-frequency raw/derived data may be retained if useful for execution-cost analysis or future work, but the first hypothesis will not make decisions on seconds/minutes-scale data.

## Rationale

### Why BTC and ETH

- both have extensive historical data,
- both are broadly traded on major spot venues,
- both generally provide materially deeper liquidity than most altcoins,
- their market rules and execution behavior are suitable for small retail spot orders,
- two assets allow a basic cross-asset robustness check without creating a large search universe.

This is a research-design decision, not a claim that BTC/ETH will produce the highest future return.

### Why 4H first

Relative to very short intervals, 4H data:

- better matches the selected swing horizon,
- produces more observations than daily-only trading,
- reduces sensitivity to microstructure noise and rapid execution,
- keeps trading costs more manageable than high-turnover minute-scale strategies,
- and remains practical for both automated monitoring and human-approved early live use.

Daily data remains useful context, but a daily-only strategy could produce too few observations for early iteration and live evaluation.

## Explicitly unresolved

This ADR does not select:

- the exchange,
- quote asset/currency,
- historical data vendor/source,
- exact trend definition,
- indicator set,
- lookback length,
- entry threshold,
- exit mechanism,
- stop distance,
- or numerical live risk limits.

## Expansion rule

Adding another asset to the development universe should require a reason beyond "more backtests". Examples of potentially valid reasons include:

- testing robustness in a meaningfully different liquid market,
- a specific hypothesis about market structure,
- or evidence that the current universe is too narrow to answer a defined research question.

Any expansion must preserve the distinction between exploratory evidence and validation evidence.

## Revisit conditions

Revisit this ADR if:

- venue availability makes BTC or ETH unsuitable,
- required historical/live data quality is inadequate,
- the 4H horizon creates an unexpectedly poor cost/sample-size tradeoff,
- or Phase B experiments demonstrate a specific reason another timeframe/universe is required.
