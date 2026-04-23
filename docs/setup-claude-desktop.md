# Setup — Claude Desktop

Two paths: **one-click install via the `.mcpb` bundle** (recommended) or manual
JSON config.

## Path A — One-click `.mcpb` bundle (recommended)

1. Download [`finturb-analytics.mcpb`](https://www.finturb.com/static/finturb-analytics.mcpb)
   (3.2 MB).
2. Double-click the downloaded file.
3. Claude Desktop opens the install dialog — click **Install**.
4. Restart Claude Desktop if prompted.
5. FinTurb Analytics now appears in your connector list with all 26 tools
   enumerated.

The bundle includes a local ESM proxy that bridges the Streamable HTTP server
to Claude Desktop's stdio transport. No Node.js, no terminal, no JSON editing.

## Path B — Manual config

### macOS

Config lives at `~/Library/Application Support/Claude/claude_desktop_config.json`.

```json
{
  "mcpServers": {
    "finturb-analytics": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp-mkic.pythonanywhere.com/mcp"
      ]
    }
  }
}
```

### Windows

Config lives at `%APPDATA%\Claude\claude_desktop_config.json`.

Same JSON as above.

### Restart

After editing the config, **fully quit** Claude Desktop (Cmd-Q on macOS, right-click
the tray icon → Quit on Windows) and relaunch. Look for FinTurb Analytics in
the plug icon at the bottom of the input box.

## Attaching the FinTurb resources

Once connected, three attachable resources become available. Click the
attachment menu inside a conversation and you'll see:

- `finturb://daily/risk-snapshot`
- `finturb://daily/market-brief`
- `finturb://methodology/overview`

Pinning `methodology/overview` at the start of a long session gives the model
the full framework without needing additional tool calls.

## Troubleshooting

- **Bundle shows "unsigned"** — Apple's `mcpb sign --self-signed` CLI reports
  success but Claude Desktop flags it as unsigned in the install dialog.
  Confirm you downloaded from `www.finturb.com/downloads/` before accepting.
- **Connector appears but tools are empty** — relaunch the app fully; the stdio
  bridge takes a moment to initialise on first install.
- **Proxy errors in the console** — usually a transient network issue with the
  remote server. Retry the tool call; if it persists, email tk@intellicore.ai.