# Example — Cross-pillar market briefing

**User prompt:**

> *"Give me a full market briefing — what should I be aware of today?"*

## Tools called

Claude (or any MCP-compliant client) routes this through `get_market_briefing`,
which is a single synthesis call aggregating seven underlying tools:

- `get_risk_score` — composite risk and 4-way liquidity-gated extension
- `get_absorption_ratio` — three-tier systemic fragility alerts
- `get_turbulence_score` — daily and rolling turbulence percentiles
- `get_sentiment_alerts` — sentiment outliers with |z| > 2
- `get_global_liquidity` — GLI composite and cycle phase
- `get_oversold_opportunities` (top 3) — cheapest names
- `get_overbought_opportunities` (top 3) — most stretched names

## Condensed response (what Claude narrates back)

> **Regime:** *<regime label>* (risk score X/100, Nth day in the regime).
> The 4-way liquidity-gated score is X/100 — *<label>*, with a liquidity boost
> of +X points on top of the base 3-way score driven by <tight policy /
> private / cross-border liquidity>.
>
> **Fragility:** *<alert level>* — N of 3 tiers active. Watch tier at the Xth
> percentile, Warning tier at the Xth, Crisis tier at the Xth. Classification:
> *<High / Above Average / Below Average / Low>*.
>
> **Turbulence:** daily EWA at the Xth percentile (Q<n>, *<Risk-On / Neutral /
> Financial Turbulence>*); rolling 10-day at the Xth.
>
> **Liquidity:** GLI X/100, cycle phase *<Calm / Speculation / Turbulence /
> Rebound>*. PLI X, PSI X, XFI X *(nowcast / measured)*.
>
> **Sentiment outliers:** N assets with |z| > 2 — notable moves on *<ticker
> list>*.
>
> **Top mean-reversion candidates:**
> - **Oversold:** *<top 3 tickers with scores>*
> - **Overbought:** *<top 3 tickers with scores>*
>
> *Informational only — not investment advice.*

## Why it works

The `summary` field on each underlying tool lets the model reason from a dense
one-line headline; the structured body supplies precise numbers for any
follow-up. The `generation_mode: synthesized` flag on `get_market_briefing`
tells the assistant to cross-check numeric claims against the underlying
deterministic tools if pressed for detail.

## Follow-ups Claude will handle naturally

```
Dig into the oversold names — which ones have the best risk-reward given the
  current regime?

Is the fragility alert consistent with what media sentiment is saying?

What's the Markov probability we stay in this turbulence state over the next
  10 days?
```
