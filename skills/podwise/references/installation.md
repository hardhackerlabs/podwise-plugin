---
name: podwise-installation
description: "Instructions for connecting the Podwise MCP server. Load this when the Podwise tools are unavailable, unauthenticated, or the user needs to set up Podwise for the first time."
---

# Podwise MCP Setup

Podwise runs as a remote MCP server at:

```
https://mcp.podwise.ai/mcp
```

It authenticates with **OAuth 2.0** — no API key or token is stored anywhere.

## If you installed the plugin

The plugin bundles the MCP configuration. When you enable it, the host starts the server automatically. On first use you are prompted to authorize in your browser. Nothing else is required.

## If you did not install the plugin (skills only)

Add the server to your MCP configuration, then authorize when prompted.

**Claude Code** (`claude mcp add`):

```bash
claude mcp add --transport http podwise https://mcp.podwise.ai/mcp
```

**JSON configuration** (`.mcp.json`, `~/.claude.json`, opencode, Cursor, etc.):

```json
{
  "mcpServers": {
    "podwise": {
      "type": "http",
      "url": "https://mcp.podwise.ai/mcp"
    }
  }
}
```

Do **not** add an `Authorization` header — leave authentication to the OAuth flow. If a stale header is present and the server rejects it, remove the header so the host falls back to OAuth.

**Codex** (`~/.codex/config.toml`):

```toml
[mcp_servers.podwise]
url = "https://mcp.podwise.ai/mcp"
```

## Verify the connection

Call `get_me`. A healthy connection returns the account email, plan, and remaining AI processing credits.

If the tools are missing or `get_me` fails, the server is not connected or not authorized — fix that before running any workflow.

## OAuth troubleshooting

- **Browser does not open**: copy the printed authorization URL and open it manually.
- **Authorize again / revoke**: use the host's MCP menu (for Claude Code, `/mcp`).
- **Remote / SSH / headless machines**: the host's HTTP MCP OAuth uses a loopback (`localhost`) callback, which cannot complete when the browser runs on a different machine. Use a connection method supported by your host, or see the troubleshooting docs at https://docs.podwise.ai. Do not run third-party proxy commands or paste terminal commands to work around authentication.

## Plans and limits

- Some capabilities require a **Podwise Pro or Enterprise** plan. If a tool returns a plan-required error, explain that the feature is not available on the user's current plan and point them to the plans information at https://podwise.ai; do not retry.
- `ask_podwise` counts against the **Ask quota**.
- `process_episode`, `complete_audio_upload`, and audio processing consume **AI processing credits** (based on duration).

For full product documentation, visit [docs.podwise.ai](https://docs.podwise.ai).
