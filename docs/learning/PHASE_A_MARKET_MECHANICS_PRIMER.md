# Phase A Market Mechanics Primer

**Purpose:** establish the minimum market knowledge needed to specify, research and later execute EdgePilot's first low-turnover spot strategy correctly.

This is not a generic trading textbook. It defines the concepts EdgePilot must model or reason about before backtest results can be trusted.

---

## 1. Spot market and trading pairs

A spot trade exchanges one asset for another without borrowing.

For a pair written as:

`BTC/USDT`

- **BTC** is the **base asset**.
- **USDT** is the **quote asset**.
- A price of `60,000 USDT` means one BTC costs 60,000 USDT.

Buying BTC/USDT means spending the quote asset to acquire the base asset. Selling BTC/USDT means exchanging BTC back into USDT.

EdgePilot's initial spot-only scope means it does not need to model liquidation, funding rates or borrowed exposure.

### Important consequence

A long-only spot strategy can be:

- exposed to the base asset, or
- out of the position and holding the quote asset.

It cannot simply profit from falling prices by opening a conventional short without introducing borrowing/derivative mechanics that are currently outside scope.

---

## 2. Order book, bid, ask and spread

At any moment an order book contains prices where participants are willing to buy or sell.

- **Bid:** highest currently available buy price.
- **Ask:** lowest currently available sell price.
- **Spread:** `ask - bid`.

The displayed last-traded price is not necessarily the exact price at which our next order can execute.

### Why EdgePilot cares

Backtests that assume every trade executes exactly at a candle's close can overstate performance. Real orders interact with bid/ask prices and available liquidity.

---

## 3. Market orders

A market order prioritizes immediate execution rather than a specific price.

Advantages:

- simple,
- high execution certainty in liquid markets.

Costs/risks:

- generally consumes liquidity and therefore tends to pay the taker fee,
- execution can occur across multiple price levels,
- final average fill can be worse than the observed reference price.

For a low-frequency system, market orders may still be entirely reasonable if the strategy's expected move is large enough relative to all execution costs. We must measure rather than assume.

---

## 4. Limit orders

A limit order specifies the worst price the trader is willing to accept.

Example:

If BTC is trading near 60,000, a buy limit at 59,800 says:

> Buy at 59,800 or lower, but do not pay more.

Potential advantages:

- price control,
- may add liquidity and qualify for maker fees.

Potential disadvantages:

- may never fill,
- may partially fill,
- a backtest that assumes every touched limit order fills is unrealistic,
- the market may move away while the order waits.

A lower explicit fee is not automatically a better total execution result.

---

## 5. Stop-style orders

A stop condition is intended to trigger an action after a price threshold is reached.

Two ideas must remain distinct:

1. **strategy invalidation** — the price/condition at which the trade thesis is considered wrong,
2. **exchange order behavior** — how an actual stop or stop-limit instruction is triggered and filled.

A stop price does not guarantee the final execution price. During rapid movement or gaps between available order-book levels, realized loss can exceed the planned amount.

---

## 6. Maker and taker

- A **maker** order rests on the book and adds liquidity before execution.
- A **taker** order executes against liquidity already resting on the book.

Exchanges often charge different fees for the two.

As observed during Phase A research, current published schedules vary materially across venues and fee tiers. Therefore EdgePilot must not hard-code a universal `0.1%` fee assumption.

The eventual market adapter should provide current pair-specific order rules and fee assumptions to the research/execution layers.

---

## 7. Slippage

**Slippage** is the difference between the intended/reference execution price and the actual average fill price.

Causes include:

- spread,
- insufficient quantity at the best price,
- market movement while the order is transmitted/executed,
- large order size relative to available liquidity.

For $100–$200 capital on a highly liquid major pair, market-impact slippage from our own size is likely to be small compared with institutional orders, but spread and timing slippage still exist and must not be modeled as zero by default.

---

## 8. Liquidity

Liquidity is not merely "high volume". For our purposes it concerns whether useful size can be bought or sold near the reference price without excessive cost or delay.

Relevant observations include:

- bid/ask spread,
- depth near the best prices,
- traded volume,
- order-book resilience,
- time-of-day / regime changes.

For the first strategy, choosing highly liquid major markets lets EdgePilot postpone deeper microstructure modeling while still recording execution costs honestly.

---

## 9. OHLCV candles

A candle for a time interval normally contains:

- **O**pen — first trade price in the interval,
- **H**igh — highest trade price,
- **L**ow — lowest trade price,
- **C**lose — final trade price,
- **V**olume — traded amount under the venue/data provider's definition.

Example: a 4-hour candle compresses all trades during four hours into one record.

### Critical limitation

OHLCV does **not** reveal the exact order in which high and low occurred inside the candle.

If a strategy's stop and profit target are both inside the same candle, candle data alone may not establish which would have executed first.

This is one reason our initial strategy should avoid unnecessary intrabar complexity.

---

## 10. Timeframe

A timeframe is both a data aggregation decision and part of the strategy's semantics.

A 4-hour strategy and a 1-minute strategy are not the same rules run at different speeds. Changing timeframe changes:

- noise level,
- trade frequency,
- typical move magnitude,
- cost significance,
- reaction speed,
- sample count,
- and potentially the existence of the edge itself.

ADR-0002 selects an hours-to-days research horizon but intentionally does not yet freeze the exact candle interval.

---

## 11. Return

A simple percentage return from entry to exit is:

`return = (exit_price - entry_price) / entry_price`

For a long position, positive values are gains and negative values are losses before costs.

Net strategy return must account for:

- entry fee,
- exit fee,
- spread,
- slippage,
- and any other execution cost actually relevant to the venue.

---

## 12. Volatility

Volatility describes the magnitude/dispersion of price movement; it is not synonymous with direction.

Two markets can both have zero net change over a day while one moves smoothly and the other swings violently.

Why EdgePilot needs it:

- stop distances should not be interpreted independently of normal price movement,
- position size can be misleading without considering volatility,
- strategy behavior often differs in quiet versus turbulent regimes.

---

## 13. Risk per trade

For EdgePilot, **risk per trade** should mean the planned loss if the defined invalidation/stop is reached, not simply the amount of money used to buy the asset.

If designated capital is `C` and accepted risk fraction is `r`:

`planned_risk = C * r`

If entry is `E` and stop is `S` for a long trade, the fractional stop distance is:

`stop_fraction = (E - S) / E`

Ignoring costs for the moment:

`position_notional = planned_risk / stop_fraction`

### Example

With $150 designated capital and a proposed 0.5% risk limit:

`planned_risk = 150 * 0.005 = $0.75`

If the stop is 3% below entry:

`position_notional = 0.75 / 0.03 = $25`

So a $25 position can represent only $0.75 of planned price risk if the strategy exits around a 3% adverse move.

This distinction is fundamental:

> **Position size is exposure; stop distance converts exposure into planned trade risk.**

Real loss can still exceed planned risk due to execution gaps/slippage, so it is not a guarantee.

---

## 14. Risk/reward ratio

If a trade plans to risk $1 to pursue a $2 gain, the nominal reward/risk is 2:1, or `2R` reward for `1R` risk.

But a high reward/risk target alone does not make a strategy good. A strategy can set an enormous target that almost never occurs.

Risk/reward must be combined with outcome probabilities.

---

## 15. Expectancy

A useful simplified formulation is:

`expectancy = P(win) * average_win - P(loss) * average_loss`

Example:

- win rate = 40%,
- average win = 2R,
- loss rate = 60%,
- average loss = 1R.

Then:

`0.40 * 2R - 0.60 * 1R = +0.20R per trade`

A strategy can therefore be profitable with a win rate below 50%, and a strategy can lose money with a high win rate if its occasional losses are much larger than its wins.

EdgePilot should optimize for evidence of positive **net expectancy**, not for the psychological appeal of a high win rate.

---

## 16. Drawdown

Drawdown measures decline from a previous equity peak.

If account equity reaches $150 and later falls to $135 before recovering:

`drawdown = (135 - 150) / 150 = -10%`

Maximum drawdown is one of the key risk measures because two strategies with the same final profit can expose the user to radically different paths and risk of abandonment/ruin.

---

## 17. Why a fixed daily-profit target is structurally wrong

A market does not owe EdgePilot one trade per day or one profitable opportunity per day.

Forcing a target such as `$1/day` can create pressure to:

- trade when no setup exists,
- increase size after a loss,
- lower entry standards,
- turn a low-frequency strategy into a high-turnover one,
- confuse short-term randomness with system quality.

A no-trade day is valid. A losing day is compatible with a positive-expectancy strategy. Evaluation belongs at the strategy/sample level, not a daily income quota.

---

## 18. Cost-adjusted trade accounting

Every evaluated trade should ultimately retain at least:

- intended signal time,
- intended/reference entry,
- actual/simulated entry,
- position size,
- intended invalidation/stop,
- intended exit logic,
- actual/simulated exit,
- entry fee,
- exit fee,
- estimated/observed slippage,
- gross P&L,
- net P&L,
- return in percent,
- return in `R`,
- strategy/configuration version,
- and evidence provenance.

A backtest that reports only price return is insufficient for deployment decisions.

---

## 19. Common backtest errors relevant to the first system

### Look-ahead bias

Using information that would not have been known at decision time.

Example: entering at a candle's open based on that same candle's final close.

### Survivorship/selection bias

Choosing assets today and pretending they were obviously the correct universe throughout historical testing.

### Overfitting

Trying enough indicators/thresholds until one happens to fit historical noise.

### Unrealistic fills

Assuming orders always execute at ideal prices whenever a candle touches them.

### Ignoring costs

Treating gross return as net return.

### Data leakage between development and validation

Repeatedly checking supposedly unseen data while tuning the strategy until it is no longer genuinely unseen.

These are research-integrity failures, not minor implementation details.

---

## 20. The first EdgePilot trade lifecycle

Conceptually, one candidate trade should eventually flow through:

`Market data -> strategy observation -> candidate setup -> risk sizing -> proposal -> human approval -> execution/paper execution -> monitoring -> exit -> journal -> evaluation`

The strategy does not directly own the wallet and does not decide its own acceptable risk.

---

## 21. What this primer permits us to decide next

We now have enough mechanical foundation to narrow:

1. initial market universe,
2. candidate timeframe range,
3. exchange/data requirements,
4. initial numerical risk policy,
5. minimum data fields and costs required by the Phase B research engine.

It does **not** yet justify choosing an indicator or claiming an edge.
