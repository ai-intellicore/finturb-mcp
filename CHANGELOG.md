# Changelog

All notable changes to the FinTurb Analytics MCP server will be documented here.
This project adheres to [Semantic Versioning](https://semver.org/).

## [1.0.0] — 2026-04-16

### Added
- Initial public release of the FinTurb Analytics MCP server.
- **26 tools across 8 signal pillars:** composite risk, financial turbulence,
  systemic fragility (including per-asset PC1/PC2 loadings history), media
  sentiment, global liquidity, statistical arbitrage, AI-generated
  intelligence, and supporting analytics.
- **3 attachable resources:** `finturb://daily/risk-snapshot`,
  `finturb://daily/market-brief`, `finturb://methodology/overview`.
- Institutional-grade output contract on every tool response:
  - `summary` — one-line human-readable headline.
  - `generation_mode` — `deterministic` or `synthesized` provenance flag.
  - `file_modified` — ISO timestamp of the underlying data file.
  - `structuredContent` — typed JSON body published by FastMCP.
- Remote MCP server with Streamable HTTP transport.
- 50 complimentary queries per 48-hour window for anonymous users; premium
  tiers available at https://www.finturb.com/subscribe.
- Public documentation repository (this repo).

### Endpoint
- **Server:** `https://mcp-mkic.pythonanywhere.com/mcp`
- **Transport:** Streamable HTTP (SSE-compatible)
- **Authentication:** None by default (IP-based rate limiting); optional
  Bearer token for premium and institutional tiers.
