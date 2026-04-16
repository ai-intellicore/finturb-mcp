# Setup — Claude Code (CLI)

One command.

```bash
claude mcp add finturb --transport sse https://mcp-mkic.pythonanywhere.com/mcp
```

Verify:

```bash
claude mcp list
```

You should see `finturb` listed with its tools enumerated. Then inside any
Claude Code session, invoke the tools directly or let the model discover them
when prompted. Example:

```
> What's the current market risk regime?
```

Claude Code will surface the tool call inline in the conversation and return
the structured response.

## Resources

Claude Code supports MCP resources natively. List them:

```bash
claude mcp resources list --server finturb
```

Read one:

```bash
claude mcp resources read --server finturb --uri finturb://methodology/overview
```

## Removing the connector

```bash
claude mcp remove finturb
```

## Troubleshooting

- **"Server not responding"** — the remote server cold-starts on the first
  request after an idle period. Retry once; subsequent calls are fast.
- **Tool count looks low** — `claude mcp list` shows a summary count; the full
  26-tool enumeration is visible once you call one from inside a session.
- **Bearer token** — if you've upgraded to a premium tier, append
  `--header "Authorization: Bearer <YOUR_TOKEN>"` to the `add` command.
