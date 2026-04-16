# Example — Oversold opportunity scan

**User prompt:**

> *"What's cheap right now and likely to bounce back?"*

## Tools called

Claude typically chains two tools here:

- `get_risk_score` — to establish the current regime (so the narrative can be
  regime-aware)
- `get_oversold_opportunities` — ranked list of mean-reversion long candidates

If the user follows up on a specific name, Claude will also call
`get_ticker_metrics` to pull the full 16-field profile.

## Condensed response

> The market is currently in a *<regime>* regime (risk X/100). Against that
> backdrop, the top oversold mean-reversion candidates ranked by composite
> opportunity score are:
>
> | Rank | Ticker | Score | Z-score | RSI | Half-life | CVaR 95 |
> |---|---|---|---|---|---|---|
> | 1 | … | … | … | … | … d | …% |
> | 2 | … | … | … | … | … d | …% |
> | 3 | … | … | … | … | … d | …% |
>
> These are raw rankings — every security surviving the upstream filters is
> included, with no minimum score threshold. The opportunity score blends
> z-score extremity, RSI, Bollinger %B, mean reversion tendency (ADF, Hurst,
> half-life), and tail risk (CVaR).
>
> Before positioning, consider the regime context: in *<stress / crisis>*
> regimes, even strongly oversold names can continue lower as correlations
> spike; in *<normal / elevated>* regimes, mean reversion tends to play out
> faster.
>
> *Informational only — not investment advice.*

## Layered filtering in the assistant layer

The `get_oversold_opportunities` response is intentionally unfiltered — no
sector or regime overlays are applied server-side. That's by design: the
assistant is expected to apply the appropriate overlay per the user's intent.
For example:

- *"Which oversold names are defensive and lagged today's rally?"* → Claude
  filters the ranked list to utilities, staples, infra.
- *"Which have the strongest mean reversion and lowest tail risk?"* → Claude
  sorts by half-life × 1/CVaR.
- *"Which ones would do well if the AR de-escalates?"* → Claude overlays the
  conditional returns table.

## Follow-ups

```
Give me the full quantitative profile for the #1 name.

Filter the list to defensive sectors that lagged Monday's risk-on rally.

Which of these have supportive media sentiment?
```
