# EdgePilot Repository Governance

This file defines the default operating rules for human and AI-assisted work in this repository.

## 1. Source-of-truth order

When sources disagree, use this order unless a newer accepted decision explicitly supersedes it:

1. current code and executable tests for implemented behavior,
2. accepted ADRs in `docs/decisions/`,
3. controlling project and risk documents,
4. current working memory,
5. historical notes and experiments.

A historical result remains evidence about what happened at that revision; it must not silently be treated as evidence about a changed implementation, dataset, schema, market regime, or strategy.

## 2. Required work loop

For each meaningful cycle:

1. **Orient** — inspect current state, relevant decisions, assumptions, and evidence.
2. **Define** — state the question/responsibility and what would count as sufficient proof.
3. **Act** — research, implement, or run the experiment without broadening scope unnecessarily.
4. **Verify** — test the result against explicit acceptance criteria and relevant failure cases.
5. **Record** — update decisions, experiment records, and current working memory.
6. **Explain** — make the result understandable: what changed, why, what was learned, and what remains unknown.

Do not mark work complete merely because code exists or a command ran successfully.

## 3. Evidence discipline

Keep these concepts distinct:

- **Observation:** something directly measured or retrieved.
- **Interpretation:** what an observation may mean.
- **Hypothesis:** a testable proposed relationship or trading edge.
- **Evaluation:** a controlled assessment of a hypothesis.
- **Decision:** an accepted project or deployment choice based on available evidence.
- **Unknown:** something not established by current evidence.

Do not convert missing evidence into a negative fact. Do not convert a backtest into proof of future profitability.

Every material strategy result should eventually be traceable to the relevant code revision, data window/source, configuration, costs/fees assumptions, and evaluation method.

## 4. Trading safety boundaries

- No live-money execution is introduced merely to accelerate learning.
- Live deployment requires the gates defined in `docs/RISK_POLICY.md`.
- Human approval remains the default for early live trading.
- Leverage, margin, derivatives, borrowing, martingale-style loss recovery, and uncapped position sizing are outside the initial scope.
- A system may and often should emit **no trade** when evidence or risk conditions are insufficient.
- Profit targets must not override risk limits or force trades.

## 5. Secrets and exchange access

Never commit API keys, private keys, seed phrases, passwords, exchange credentials, or unrestricted tokens.

When exchange integration eventually begins, use least-privilege credentials. Withdrawal permission should be disabled for trading automation unless an explicitly reviewed future requirement proves otherwise.

## 6. Change discipline

- Prefer one coherent responsibility per implementation/research cycle.
- Record durable architecture/product decisions as ADRs.
- Keep experimental conclusions tied to their exact assumptions and revision.
- Do not add ML, LLMs, agents, extra indicators, extra exchanges, or additional strategies merely because they are available.
- New complexity must answer a defined question or close a demonstrated gap.

## 7. Working memory

`docs/working-memory/CURRENT_STATE.md` should answer:

- Where are we now?
- What has been accepted?
- What remains unresolved?
- What is the next coherent responsibility?

Keep it current and concise. Detailed history belongs elsewhere.
