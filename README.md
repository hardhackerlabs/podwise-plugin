# Podwise Plugin

[![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-6B4FBB)](https://code.claude.com/docs/en/plugins)
[![Codex](https://img.shields.io/badge/Codex-plugin-000000)](https://developers.openai.com/codex)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

Turn podcast episodes into AI-powered insights from inside Claude Code and Codex. This plugin bundles the **Podwise Agent Skill** with the **remote Podwise MCP server** (`https://mcp.podwise.ai/mcp`), so installing the plugin wires everything up.

In short: **a podcast MCP server for AI agents.** Point any MCP client at `https://mcp.podwise.ai/mcp` and it can search podcasts and episodes, pull a full transcript, read the AI summary, chapters, highlights, Q&A and mind map, ask questions across transcripts, translate an episode, clip key moments, and export notes as Markdown or SRT.

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
| **catch-up** | Catch up on missed episodes from shows you follow |
| **weekly-recap** | Generate a weekly listening recap with highlights |
| **episode-notes** | Export episode summaries and highlights to your PKM |
| **topic-research** | Research a topic across multiple episodes |
| **episode-debate** | Challenge and stress-test ideas from an episode |
| **language-learning** | Generate language flashcards from a transcript |
| **discover** | Get personalised podcast recommendations |

## MCP server and tools

**Endpoint:** `https://mcp.podwise.ai/mcp` — Streamable HTTP, OAuth 2.1. No API key lives in this repository.
Listed in [Anthropic's plugin directory](https://claude.ai) and on the official MCP Registry as
`io.github.hardhackerlabs/podwise` (see [`server.json`](./server.json)).

38 tools, grouped:

| Group | Tools |
| --- | --- |
| Account | `get_me` |
| Search and discovery | `search_episodes`, `search_podcasts`, `get_popular_episodes` |
| Library and history | `list_followed_episodes`, `list_followed_podcasts`, `list_podcast_episodes`, `list_my_episodes`, `set_podcast_follow`, `set_episode_read` |
| Processing and import | `process_episode`, `import_episode`, `start_audio_upload`, `complete_audio_upload` |
| Artifacts and Q&A | `get_episode`, `get_episode_summary`, `get_episode_transcript`, `ask_podwise`, `get_ask_history`, `translate_episode`, `list_episode_translations` |
| Clips | `create_clip`, `get_clip`, `list_clips`, `delete_clip`, `export_clips_markdown`, `send_clips` |
| Export | `export_episode_markdown`, `export_episode_srt`, `send_episode` |
| Enterprise | `enterprise_process_audio`, `enterprise_get_status`, `enterprise_get_result`, `enterprise_query_results`, `enterprise_translate`, `enterprise_get_translation`, `enterprise_export`, `enterprise_get_usage` |

Full parameter reference: [`skills/podwise/references/mcp.md`](./skills/podwise/references/mcp.md).

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
codex plugin add podwise@podwise
```

Then complete the OAuth flow when prompted.

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

> **Remote / SSH note:** the host's HTTP MCP OAuth uses a loopback callback, which does not work when the host runs on a machine different from your browser. Use a connection method supported by your host, or see the troubleshooting docs at https://docs.podwise.ai. Do not run third-party proxy commands to work around authentication.

## Repository layout

```
podwise-plugin/                            # this repository is the plugin (source: "./")
├── .claude-plugin/
│   ├── marketplace.json                   # Claude Code marketplace
│   └── plugin.json                        # Claude Code plugin manifest
├── .codex-plugin/plugin.json              # Codex / ChatGPT client manifest
├── .cursor-plugin/
│   ├── marketplace.json                   # Cursor marketplace
│   └── plugin.json                        # Cursor plugin manifest
├── .agents/plugins/marketplace.json       # Codex marketplace
├── plugin.json                            # Portable Agent Plugins manifest
├── mcp.json / .mcp.json                   # Remote MCP server config
├── server.json                            # MCP registry manifest (official registry, aggregators)
├── skills/podwise/                        # The Podwise skill
│   ├── SKILL.md
│   ├── references/
│   ├── workflows/
│   └── agents/openai.yaml
└── assets/
```

The MCP definition is duplicated as `mcp.json` (portable / Codex, `streamable-http`) and `.mcp.json` (Claude Code, `http`). Keep the transport type matching each platform.

## Brand assets

Directory listings use two square images declared in `plugin.json` under `extensions.com.openai.interface`:

| Asset | File | Size | Notes |
| --- | --- | --- | --- |
| Logo | `assets/logo.png` | 1024×1024 | Square, PNG, ≤5 MiB |
| Composer icon | `assets/composer-icon.png` | 256×256 | Square, PNG, shown in the chat composer |

Brand color is `#7948E8`.

## License

[MIT](./LICENSE) © Podwise
