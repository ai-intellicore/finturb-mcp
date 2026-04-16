# FinTurb Analytics MCP Server

> Institutional-grade risk analytics for AI assistants.

[![MCP](https://img.shields.io/badge/MCP-1.0-blue)](https://modelcontextprotocol.io)
[![License](https://img.shields.io/badge/license-Proprietary-lightgrey)](./LICENSE)
[![Website](https://img.shields.io/badge/website-finturb.com-00d4aa)](https://www.finturb.com)
[![Tools](https://img.shields.io/badge/tools-26-6c9eff)](./tools.json)
[![Resources](https://img.shields.io/badge/resources-3-a78bfa)](./tools.json)

---

## Overview

FinTurb Analytics is a remote Model Context Protocol (MCP) server that exposes
institutional-grade quantitative risk analytics — composite risk regimes,
three-tier systemic fragility alerts, Markov regime transition probabilities,
GDELT-derived media sentiment across 27+ assets, a global liquidity index with
regional decomposition, and a statistical arbitrage scanner covering 550+
securities — as structured tools any MCP-compliant AI assistant can call
autonomously during a conversation.

## Quick start

```
Server URL:    https://mcp-mkic.pythonanywhere.com/mcp
Transport:     Streamable HTTP (SSE-compatible)
Auth:          None required — IP-based rate limiting
Free tier:     50 calls / 48 hours
Premium:       https://www.finturb.com/subscribe
```

No account, no API key, no installation — paste the URL into any MCP-compatible
client and start asking questions.

## Setup

| Client | Guide |
|---|---|
| Claude.ai (web) | [docs/setup-claude.md](./docs/setup-claude.md) |
| ChatGPT (Developer Mode) | [docs/setup-chatgpt.md](./docs/setup-chatgpt.md) |
| Claude Desktop (macOS / Windows) | [docs/setup-claude-desktop.md](./docs/setup-claude-desktop.md) |
| Claude Code (CLI) | [docs/setup-claude-code.md](./docs/setup-claude-code.md) |

## Output contract

Every tool response follows the same shape so an AI can reason from the summary
line alone, verify numbers in the structured body, and decide how much to trust
the response from the provenance flag.

```json
{
  "summary":         "3-way risk 18.4/100 — regime normal (1d). 4-way 27.0/100 …",
  "generation_mode": "deterministic",
  "...": "structured fields",
  "file_modified":   "2026-04-14T03:41:01Z"
}
```

- `summary` — one-line human-readable headline.
- `generation_mode` — `deterministic` for pure data reads, `synthesized` for
  payloads containing LLM-generated text or multi-pillar synthesis.
- `file_modified` — ISO timestamp of the underlying data file so callers can
  assess freshness at the point of use.

## Tool reference

Full machine-readable list in [`tools.json`](./tools.json); detailed descriptions
in [`docs/tool-reference.md`](./docs/tool-reference.md).

### Composite risk monitor
| Tool | One-liner |
|---|---|
| `get_risk_score` | Composite risk score (0–100) with regime, decomposition, and 4-way liquidity-gated extension. |
| `get_risk_history` | Daily risk score, regime label, and duration for the last N days. |
| `get_conditional_returns` | Historical return statistics by regime per core asset (5d / 21d horizons). |
| `get_interaction_table` | 2×2×2 conditional returns table across three signal dimensions for a given asset. |

### Financial turbulence
| Tool | One-liner |
|---|---|
| `get_turbulence_score` | Daily and 10-day rolling turbulence percentiles with regime labels. |
| `get_transition_probabilities` | Markov transition matrices for daily and rolling turbulence states. |
| `get_signal_dates` | Historical bearish (Type A) and bullish (Type B) signal dates with outcomes. |

### Systemic fragility
| Tool | One-liner |
|---|---|
| `get_absorption_ratio` | Three-tier fragility alert system (30d Watch / 60d Warning / 90d Crisis). |
| `get_fragility_loadings` | 30-day fragility history tail — aggregate time series. |
| `get_pc_loadings_history` | Per-asset PC1/PC2 eigenvector loadings across 30/60/90-day windows. |

### Media sentiment
| Tool | One-liner |
|---|---|
| `get_media_sentiment` | Per-asset tone, volume, and composite signal (27+ assets). |
| `get_sentiment_heatmap` | Full sentiment ranking across every tracked asset. |
| `get_sentiment_alerts` | Only assets with |z| > 2 — actionable outliers. |
| `get_geopolitical_tone` | Goldstein-scale global geopolitical tension indicator. |

### Global liquidity
| Tool | One-liner |
|---|---|
| `get_global_liquidity` | GLI (0–100) composite with PLI / PSI / XFI sub-indices and cycle phase. |
| `get_liquidity_history` | Monthly GLI + sub-indices time series with cycle phase per observation. |
| `get_regional_liquidity` | Per-region liquidity readings (US, Eurozone, China, Japan, UK). |

### Statistical arbitrage
| Tool | One-liner |
|---|---|
| `get_oversold_opportunities` | Oversold candidates ranked by composite opportunity score. |
| `get_overbought_opportunities` | Overbought candidates ranked by composite opportunity score. |
| `get_ticker_metrics` | Full 16-field quantitative profile for any of 550+ tickers. |
| `get_stat_arb_summary` | Universe-wide oversold/overbought counts plus top 5 each direction. |

### AI-generated intelligence (`synthesized`)
| Tool | One-liner |
|---|---|
| `get_market_briefing` | One-call cross-pillar synthesis. |
| `get_gpt_commentary` | GPT-4o narrative commentary on transitions and daily asset moves. |
| `get_signal_strategist` | Dashboard-grade snapshot with 4-way liquidity gate. |

### Supporting analytics
| Tool | One-liner |
|---|---|
| `get_periodic_returns` | Annual returns for 20 asset classes (2018–present). |
| `get_stablecoin_scorecard` | RAG assessment across 10 dimensions per major stablecoin. |

## Resources

Compliant clients (Claude Desktop, Claude Code, Cursor) can pin these documents
into the conversation context so the model doesn't need to re-issue tool calls.

- `finturb://daily/risk-snapshot` — today's composite risk score + 4-way
  liquidity-gated extension *(deterministic)*
- `finturb://daily/market-brief` — today's cross-pillar market briefing
  *(synthesized)*
- `finturb://methodology/overview` — static reference covering the eight
  pillars, data sources, and the output contract

## Example conversations

Three worked examples live in [`examples/`](./examples):

- [`example-market-briefing.md`](./examples/example-market-briefing.md) —
  "Give me a full market briefing."
- [`example-oversold-scan.md`](./examples/example-oversold-scan.md) —
  "What's cheap right now and likely to bounce back?"
- [`example-risk-assessment.md`](./examples/example-risk-assessment.md) —
  "How risky are markets right now compared to last week?"

## Rate limits

| Tier | Quota | Notes |
|---|---|---|
| Anonymous | 50 calls / 48 h | IP-based, no account required. |
| Premium | Unlimited | Bearer token. [Subscribe](https://www.finturb.com/subscribe). |
| Institutional | Unlimited | Custom arrangements — contact tk@intellicore.ai. |

## Data freshness

All FinTurb analytics are refreshed daily on a scheduled pipeline running
between 00:05 and 04:05 UTC, drawing from established primary sources — market
data, macro statistics, and open-source news datasets. Most signals reflect the
previous market close (T+0); a small number carry a one-day publication lag or
mixed recency where the underlying macro series are published weekly to
quarterly. Every MCP tool response carries a `file_modified` ISO timestamp so
callers can verify freshness at the point of use. Signal methodologies and
inputs are proprietary to AI IntelliCore Limited.

## About AI IntelliCore

AI IntelliCore Limited is a Cyprus-registered quantitative analytics firm
building institutional-grade risk intelligence for AI-native workflows.
Learn more at [finturb.com](https://www.finturb.com) and
[riskregime.com](https://www.riskregime.com).

## Support

- **Email:** tk@intellicore.ai
- **Issues:** [github.com/ai-intellicore/finturb-mcp/issues](https://github.com/ai-intellicore/finturb-mcp/issues)
- **Website:** [finturb.com/mcp-access](https://www.finturb.com/mcp-access)

## License

Proprietary. See [LICENSE](./LICENSE) for terms.

---

*© 2026 AI IntelliCore Limited. FinTurb and the AI IntelliCore logo are trademarks
of AI IntelliCore Limited. The Model Context Protocol is a trademark of
Anthropic PBC.*
