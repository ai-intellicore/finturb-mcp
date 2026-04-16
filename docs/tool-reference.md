# Tool Reference

Full reference for all 26 tools exposed by the FinTurb Analytics MCP server.
For the machine-readable equivalent see [`../tools.json`](../tools.json).

**Output contract** — every tool returns a dict with:

```
summary         one-line human-readable headline
generation_mode "deterministic" | "synthesized"
...<structured fields>...
file_modified   ISO timestamp of the underlying data file
```

FastMCP publishes the dict as `structuredContent` in the MCP envelope.

---

## Composite risk monitor

### `get_risk_score`

**Category:** `composite_risk` · **Mode:** `deterministic`

Current composite risk score (0–100) with regime classification (Normal /
Elevated / Stress / Crisis), regime duration, AR / turbulence / GDELT
decomposition, fragile-shock status, count of elevated signals, and the 4-way
liquidity-gated extension with GLI / PLI / PSI / XFI readings.

**Parameters:** none.

**Example prompt:** *"What's the current market risk regime?"*

**Response shape:**

```json
{
  "summary": "3-way risk X/100 — regime <label> (Nd). 4-way X/100 — <label>. …",
  "generation_mode": "deterministic",
  "data_as_of": "YYYY-MM-DD",
  "risk_score": 0.0,
  "regime": "…",
  "risk_score_4way": 0.0,
  "regime_4way": "…",
  "rs4_decomposition": { "base_risk_score": 0, "liq_boost": 0, "fragile_kicker": 0 },
  "liquidity": { "gli": 0, "pli": 0, "psi": 0, "xfi": 0, "xfi_is_nowcast": false },
  "jt4_signals": { "...": "..." },
  "file_modified": "YYYY-MM-DDTHH:MM:SSZ"
}
```

### `get_risk_history`

Daily risk score, regime label, and regime duration for the last N days.

**Parameters:** `days` (int, default 90).
**Example prompt:** *"How has the risk regime evolved over the last 60 days?"*

### `get_conditional_returns`

Historical return statistics by regime per core asset — mean return,
P(negative), and CVaR.

**Parameters:** `horizon` ("5d" | "21d", default "5d").
**Example prompt:** *"What tends to happen to SPY in a stress regime over the next 21 days?"*

### `get_interaction_table`

2×2×2 conditional returns table across the three primary signal dimensions for
a given asset.

**Parameters:** `asset` (string, default "SPY"; options: SPY, BTC_USD, GLD, HYG, all).
**Example prompt:** *"Show me the interaction between the three signal dimensions for SPY returns."*

---

## Financial turbulence

### `get_turbulence_score`

Daily and 10-day rolling turbulence percentiles with quartile classifications
and regime labels (Risk-On / Neutral / Financial Turbulence).

**Parameters:** none.
**Example prompt:** *"How unusual are today's cross-asset moves?"*

### `get_transition_probabilities`

Markov Q1–Q3 transition matrices for daily and 10-day rolling turbulence states
— P(remain) and P(move) by state and duration bucket. Probabilities within
±3pp are statistically indistinguishable.

**Parameters:** none.
**Example prompt:** *"How long do these turbulence regimes typically last?"*

### `get_signal_dates`

Historical bearish (Type A) and bullish (Type B) signal dates with percentile,
strength, subsequent asset returns, and outcome.

**Parameters:** `signal_type` ("A" | "B" | "both", default "both").
**Example prompt:** *"Show historical analogues for the current turbulence signal."*

---

## Systemic fragility

### `get_absorption_ratio`

Three-tier alert system: 30-day Watch, 60-day Warning, 90-day Crisis — each
with independent thresholds and active-tier flags. Includes overall
classification and alert level.

**Parameters:** none.
**Example prompt:** *"Is systemic fragility triggering any alerts today?"*

### `get_fragility_loadings`

30-day Absorption Ratio history tail — aggregate time series only.

**Parameters:** none.
**Example prompt:** *"What's the recent fragility trajectory?"*

### `get_pc_loadings_history`

Per-asset PC1 / PC2 eigenvector loadings across 30/60/90-day rolling windows,
with variance_explained per date. Reveals exactly which assets are driving
systemic coupling and how that composition shifts through stress episodes.

**Parameters:**
- `window` (int, 30 | 60 | 90, default 30)
- `days` (int, 1–90, default 60)

**Example prompt:** *"Which assets are driving PC1 loadings right now, and how has that shifted over the last 60 days?"*

---

## Media sentiment

### `get_media_sentiment`

Per-asset tone, volume, and global z-scores, composite signal, momentum, and
alert flags. 27+ assets tracked.

**Parameters:** `tickers` (comma-separated string, optional).
**Example prompt:** *"What's media sentiment saying about gold and Bitcoin?"*

### `get_sentiment_heatmap`

All assets sorted by composite-signal magnitude — the full sentiment heatmap
in structured form.

**Parameters:** none.
**Example prompt:** *"Show me the full sentiment ranking across every asset."*

### `get_sentiment_alerts`

Only assets with |tone z| > 2 or |volume z| > 2 — actionable outliers.

**Parameters:** none.
**Example prompt:** *"Which assets have sentiment outliers today?"*

### `get_geopolitical_tone`

Global Goldstein Scale average and conflict ratio as a macro geopolitical
tension indicator.

**Parameters:** none.
**Example prompt:** *"What's the current global geopolitical tension level?"*

---

## Global liquidity

### `get_global_liquidity`

GLI (0–100) composite with PLI (policy), PSI (private sector), and XFI
(cross-border) sub-indices, cycle phase classification, XFI-nowcast flag, and
regional breakdown.

**Parameters:** none.
**Example prompt:** *"How tight are global liquidity conditions?"*

### `get_liquidity_history`

Monthly GLI + sub-indices time series with cycle phase per observation.

**Parameters:** `months` (int, 1–240, default 12).
**Example prompt:** *"How have liquidity conditions trended over the last two years?"*

### `get_regional_liquidity`

Per-region liquidity readings for US, Eurozone, China, Japan, UK with
comparative rankings.

**Parameters:** none.
**Example prompt:** *"How are US liquidity conditions compared to Europe, China, and Japan?"*

---

## Statistical arbitrage

### `get_oversold_opportunities`

Oversold securities ranked by composite opportunity score. Every candidate
surviving the upstream filters is returned, ranked — no minimum threshold.
Fields include z-score, RSI, Bollinger %B, mean-reversion score, half-life,
and CVaR.

**Parameters:** `top_n` (int, default 20).
**Example prompt:** *"What's cheap right now and likely to bounce back?"*

### `get_overbought_opportunities`

Overbought securities ranked by composite opportunity score. Same fields as
oversold.

**Parameters:** `top_n` (int, default 20).
**Example prompt:** *"Which names look stretched to the upside and due for a pullback?"*

### `get_ticker_metrics`

Full 16-field quantitative profile for any of 550+ tickers — z-score, RSI,
Bollinger %B, MACD, mean-reversion score, ADF, Hurst, half-life, CVaR,
volatility, and composite opportunity scoring.

**Parameters:** `ticker` (string, required).
**Example prompt:** *"Give me the full quantitative profile for NVDA."*

### `get_stat_arb_summary`

Overview: oversold and overbought counts across the universe plus the top 5
opportunities each direction.

**Parameters:** none.
**Example prompt:** *"What's at extremes today across the stat-arb universe?"*

---

## AI-generated intelligence

*All three tools in this pillar are marked `generation_mode: synthesized` —
cross-check numeric claims against the deterministic tools.*

### `get_market_briefing`

One-call cross-pillar synthesis — regime, fragility alerts, turbulence,
sentiment outliers, liquidity phase, and top stat-arb candidates. Aggregates
the outputs of seven other tools with a synthesis framing.

**Parameters:** none.
**Example prompt:** *"Give me a full market briefing."*

### `get_gpt_commentary`

GPT-4o-generated narrative commentary on transition probabilities and daily
asset moves.

**Parameters:** none.
**Example prompt:** *"What's the narrative read on today's market?"*

### `get_signal_strategist`

Dashboard-grade snapshot — regime gauge, decomposition, 4-way liquidity-gated
score, GLI / PLI / PSI / XFI readings, conditional returns, and turbulence
summary.

**Parameters:** none.
**Example prompt:** *"Give me the Signal Strategist dashboard read."*

---

## Supporting analytics

### `get_periodic_returns`

Annual returns for 20 asset classes (2018–present including YTD) with risk
metrics (Sharpe, Sortino, Omega, max drawdown).

**Parameters:** none.
**Example prompt:** *"How have asset classes performed this year versus their long-term track record?"*

### `get_stablecoin_scorecard`

Red / Amber / Green assessment across 10 dimensions per major stablecoin —
reserve model, regulatory status, transparency, custody, attestation, and
more.

**Parameters:** none.
**Example prompt:** *"Compare stablecoin risk profiles."*

---

## Resources

Attachable documents for compliant clients (Claude Desktop, Claude Code, Cursor):

| URI | Description | Mode |
|---|---|---|
| `finturb://daily/risk-snapshot` | Today's composite risk score + 4-way extension. | deterministic |
| `finturb://daily/market-brief` | Today's cross-pillar market briefing. | synthesized |
| `finturb://methodology/overview` | Static reference — eight pillars, data sources, output contract. | deterministic |
