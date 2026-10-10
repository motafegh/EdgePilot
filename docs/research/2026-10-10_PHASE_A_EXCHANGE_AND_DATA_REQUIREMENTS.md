# Phase A Research: Exchange and Data Requirements

**Date:** 2026-10-10  
**Status:** Requirements established; provider selection intentionally unresolved

## Purpose

EdgePilot now has a constrained first research problem:

- spot only,
- BTC and ETH initial universe,
- low-turnover swing operation,
- 4H primary candidate decision timeframe,
- trend following as the first strategy family.

The next question is not "which exchange is most popular?" It is:

> What must an exchange/data source provide so that EdgePilot can research and later execute this problem honestly and safely?

## Why the exchange cannot be chosen by fee alone

Current published spot fee schedules differ materially across major venues. Lower fees improve strategy economics, but fee level is only one constraint.

A candidate venue can still be unsuitable if:

- it is unavailable or restricted in the user's actual jurisdiction,
- required fiat/crypto funding routes are impractical,
- BTC/ETH quote markets are not sufficiently liquid,
- API order/data semantics are inadequate,
- API permissions cannot be restricted safely,
- minimum order rules conflict with small position sizing,
- or historical and live data cannot be reconciled.

Therefore provider selection remains unresolved until legal/operational availability is confirmed rather than inferred.

## Mandatory exchange requirements

A venue considered for eventual live EdgePilot use should satisfy all of the following.

### 1. Legal and account availability

- available to the user's actual jurisdiction,
- usable under the venue's current terms/KYC requirements,
- practical deposit/withdrawal path,
- no dependence on circumventing geographic restrictions.

This is a hard gate.

### 2. Spot BTC and ETH markets

The venue must provide sufficiently liquid spot markets for BTC and ETH against a practical quote asset/currency.

The exact quote currency should be chosen only after considering:

- funding currency,
- market depth/spread,
- fee schedule,
- tax/accounting implications,
- stablecoin-specific risk if a stablecoin quote is used.

### 3. Machine-readable market rules

EdgePilot must be able to discover or retain current rules such as:

- minimum quantity,
- minimum notional,
- quantity step size,
- price tick size,
- supported order types,
- trading status,
- and relevant rate limits.

For example, Binance exposes trading rules through `exchangeInfo`, and Kraken documents machine-readable asset/pair information and minimums. The exact adapter will be provider-specific.

### 4. Historical market data

The provider/data source must support enough trustworthy history to reconstruct at least:

- timestamp,
- open,
- high,
- low,
- close,
- volume,
- market/pair identity,
- interval definition,
- source/provenance.

4H candles may be obtained directly or derived from a smaller canonical interval, but aggregation rules must be deterministic and time-zone/interval boundaries explicit.

### 5. Live public market data

For later live paper operation, EdgePilot needs:

- current trades and/or candles,
- bid/ask or book-ticker information,
- timestamps,
- reconnect/recovery semantics,
- and a way to identify stale or missing data.

REST-only polling may be sufficient for some 4H responsibilities, but WebSocket support is desirable for reliable live monitoring and execution evidence.

### 6. Trading API

Before real-money integration, the venue should provide programmatic:

- order submission,
- order cancellation,
- order-status queries,
- fill/trade history,
- balance/account state,
- and ideally client-defined order identifiers for reconciliation/idempotency.

### 7. Restricted API permissions

The exchange must permit API credentials that do **not** have withdrawal permission.

EdgePilot's live key should have only the permissions necessary for:

- market/account reads,
- spot order placement/cancellation,
- and execution reconciliation.

Withdrawal capability is not required by the trading system and should remain disabled.

### 8. Cost observability

EdgePilot must be able to obtain or configure:

- maker fee,
- taker fee,
- pair/fee tier,
- actual charged fee from fills when available,
- spread observations,
- and later realized slippage.

Backtest configuration must retain the cost assumptions used for each experiment.

### 9. Small-account compatibility

Minimum notional/quantity rules must permit sensible risk sizing for an account in the $100–$200 range.

The project should not distort its risk policy merely to satisfy a venue's minimum order size.

If the smallest allowed position produces more planned risk than permitted, that venue/pair is unsuitable for the bounded live experiment.

## Historical-data-source requirements

The research source and eventual execution venue do not strictly have to be identical, but divergence creates basis risk.

If source A is used for backtesting and venue B for execution, EdgePilot must explicitly assess:

- candle construction differences,
- quote currency differences,
- price/spread differences,
- symbol/listing history,
- outages/missing intervals,
- fee differences,
- and whether the strategy's observed edge transfers to venue B.

Therefore the simplest initial preference is:

> **Use data from the same venue we expect to paper/live trade whenever feasible, while keeping the internal market-data model provider-neutral.**

This is a preference, not a final provider selection.

## Current candidate capability evidence

This section records capability evidence only; it is not a ranking or recommendation.

### Binance

Current official documentation exposes spot endpoints including exchange information, klines/candles, tickers, trades, account/order history and WebSocket user data. Its current published regular-user general spot fee schedule lists 0.10% maker / 0.10% taker before optional discounts.

### Kraken

Current official documentation exposes public/pre-trade market data and account/trading APIs. Kraken's API-key model separates permissions such as querying funds, modifying trades and withdrawing funds, which is compatible with least-privilege credential design. Its current lowest spot fee tier is materially higher than Binance's published regular tier, so fee impact would need explicit modeling.

### Coinbase Advanced

Current official documentation states that Advanced Trade supports programmatic trading/order management through REST and real-time market data through WebSocket. Fees are tier-dependent and the current applicable rate should be obtained from the account/order context rather than assumed globally.

## What remains required before selecting a venue

The project still needs the user's actual operational jurisdiction/account-access constraints. That information materially changes which exchanges can legally and practically be used.

Until that is established, EdgePilot should **not** encode one exchange as the product's default.

## Architectural consequence

Phase B should define provider-neutral domain records first, then place exchange-specific behavior behind adapters.

At minimum, the core should not depend directly on provider-specific symbol spellings or raw response formats.

Conceptually:

`Provider adapter -> canonical market data / market rules -> research engine`

and later:

`proposal -> risk approval -> execution adapter -> provider -> normalized execution record`

## Exit result

Exchange/data requirements are now sufficiently defined to build provider-neutral research infrastructure, but final provider selection remains an explicit unresolved Phase A decision because legal/account availability must not be guessed.
