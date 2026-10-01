# Podwise Plugin

[![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-6B4FBB)](https://code.claude.com/docs/en/plugins)
[![Codex](https://img.shields.io/badge/Codex-plugin-000000)](https://developers.openai.com/codex)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

Turn podcast episodes into AI-powered insights from inside Claude Code and Codex. This plugin bundles the **Podwise Agent Skill** with the **remote Podwise MCP server** (`https://mcp.podwise.ai/mcp`), so installing the plugin wires everything up — no CLI, no local server.

Podwise transforms hours of audio into transcripts, summaries, outlines, Q&A, mind maps and clips. The skill teaches your agent how to combine those tools into repeatable workflows.

## What's included

| Component | Description |
| --- | --- |
| **Skill** | `podwise` — routes your intent to the right podcast workflow |
| **MCP server** | Remote HTTP server at `https://mcp.podwise.ai/mcp` (OAuth) |

### Workflows

The skill routes your intent automatically:

| Workflow | What it does |
| --- | --- |
| **refine-taste** | Build your listener profile for personalised outputs |
| **catch-up** | Catch up on missed episodes from shows you follow |
| **weekly-recap** | Generate a weekly listening recap with highlights |
| **episode-notes** | Export episode summaries and highlights to your PKM |
| **topic-research** | Research a topic across multiple episodes |
| **episode-debate** | Challenge and stress-test ideas from an episode |
| **language-learning** | Generate language flashcards from a transcript |
| **discover** | Get personalised podcast recommendations |

## Install

### Claude Code

```
/plugin marketplace add hardhackerlabs/podwise-plugin
/plugin install podwise@podwise
```

Then authenticate the MCP server when prompted (OAuth runs in your browser via `/mcp`).

### Codex / ChatGPT

```
codex plugin marketplace add hardhackerlabs/podwise-plugin
```

Then install **Podwise** from the Plugins Directory and complete the OAuth flow.

### Other agents (skills only)

If your agent reads skills directly but does not install plugins, install the skill and add the MCP server manually:

```bash
npx skills add hardhackerlabs/podwise-plugin
```

Then add this to your MCP configuration:

```json
{ "mcpServers": { "podwise": { "type": "http", "url": "https://mcp.podwise.ai/mcp" } } }
```

## Requirements

- A Podwise account. The MCP server uses **OAuth 2.0**; no API key is stored in this repository.
- Network access to `https://mcp.podwise.ai/mcp`.
- Some capabilities require a Podwise Pro or Enterprise plan.

> **Remote / SSH note:** Claude Code's native HTTP MCP OAuth uses a loopback callback, which does not work when Claude Code runs on a machine different from your browser. In that case, bridge the remote server through stdio:
> ```bash
> npx -y mcp-remote https://mcp.podwise.ai/mcp
> ```

## Repository layout

```
podwise-plugin/
├── .claude-plugin/marketplace.json      # Claude Code marketplace
├── .agents/plugins/marketplace.json     # Codex marketplace
└── plugins/podwise/
    ├── .claude-plugin/plugin.json       # Claude Code plugin manifest
    ├── plugin.json                      # Portable Agent Plugins manifest (Codex)
    ├── mcp.json / .mcp.json             # Remote MCP server config (identical)
    ├── skills/podwise/                  # The Podwise skill
    │   ├── SKILL.md
    │   ├── references/
    │   ├── workflows/
    │   └── agents/openai.yaml
    └── assets/
```

The MCP definition is intentionally duplicated as `mcp.json` (portable / Codex) and `.mcp.json` (Claude Code default). Keep the two files identical.

## Brand assets

Directory listings use two square images declared in `plugins/podwise/plugin.json` under `extensions.com.openai.interface`:

| Asset | File | Size | Notes |
| --- | --- | --- | --- |
| Logo | `assets/logo.png` | 1024×1024 | Square, PNG, ≤5 MiB |
| Composer icon | `assets/composer-icon.png` | 256×256 | Square, PNG, shown in the chat composer |

Brand color is `#7948E8`.

## License

[MIT](./LICENSE) © Podwise
