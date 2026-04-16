# Setup — ChatGPT (Developer Mode)

Connect FinTurb Analytics to ChatGPT as a custom MCP connector.

## Prerequisites

- ChatGPT Plus, Team, or Enterprise account.
- **Developer Mode** enabled in your account settings.

## Steps

1. Open ChatGPT.
2. Click your profile → **Settings** → **Connectors** (under "Beta features" or
   "Developer" depending on plan).
3. Enable **Developer Mode** if you haven't already.
4. Click **Add custom MCP server**.
5. Fill in:
   - **Name:** `FinTurb Analytics`
   - **Server URL:** `https://mcp-mkic.pythonanywhere.com/mcp`
   - **Transport:** Streamable HTTP
   - **Authentication:** None
6. Click **Save**.
7. Start a new chat, make sure FinTurb Analytics is toggled on in the Tools
   menu, and try a prompt like:

   > *Give me a full market briefing.*

## Screenshots

![ChatGPT connector settings](../images/chatgpt-settings.png)

## Troubleshooting

- **Connector validates but tools don't appear** — some older clients cache
  the tool list. Refresh the page or start a new chat.
- **Rate limit errors** — ChatGPT routes all its users through a shared
  egress pool, which may hit the anonymous quota faster than individual use.
  Upgrade at [finturb.com/subscribe](https://www.finturb.com/subscribe) for
  unlimited access with a Bearer token.
- **Still stuck?** Email tk@intellicore.ai.
