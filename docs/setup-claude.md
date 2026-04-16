# Setup — Claude.ai (web)

Connect FinTurb Analytics to Claude.ai in under a minute. No account on our
side, no API key.

## Steps

1. Open [claude.ai](https://claude.ai) and sign in.
2. Click your name in the bottom-left corner → **Settings**.
3. Go to the **Connectors** tab.
4. Click **Add custom connector**.
5. Fill in:
   - **Name:** `FinTurb Analytics`
   - **URL:** `https://mcp-mkic.pythonanywhere.com/mcp`
6. Click **Add**.
7. Verify that FinTurb Analytics now appears in the Connectors list with its
   tools enumerated.
8. Start a **new conversation** (existing conversations don't pick up newly
   added connectors) and ask:

   > *What's the current market risk regime?*

Claude will call `get_risk_score` autonomously and narrate the response.

## Screenshots

![Connector settings](../images/claude-settings.png)
![Tools enumerated](../images/claude-tools.png)

## Troubleshooting

- **Tools don't appear** — quit and re-launch the browser, then start a brand
  new chat. Existing conversations retain the connector set from when they
  started.
- **"Connection failed" or "Invalid URL"** — confirm the URL is exactly
  `https://mcp-mkic.pythonanywhere.com/mcp` (no trailing slash, no typos).
  Pasting it into a browser should return a JSON-RPC error response, which
  confirms the endpoint is live.
- **"quota_exhausted"** — the per-IP anonymous pool is temporarily at its
  limit. Wait a few hours or upgrade at [finturb.com/subscribe](https://www.finturb.com/subscribe).
- **Still stuck?** Email tk@intellicore.ai with your client name, version, and
  the exact error message.

## What to try next

```
Give me a full market briefing.
Is systemic fragility triggering any alerts today?
What's cheap right now and likely to bounce back?
Which assets are driving PC1 loadings right now, and how has that composition
  shifted over the last 60 days?
```

See [`../examples/`](../examples/) for worked conversations.
