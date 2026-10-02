# Listener Context

Podwise personalisation is derived live from MCP data at the start of each session. There is no stored profile: never read or write a local `taste.md`, and never carry preferences across sessions.

## Derive context from MCP

Call these tools before the workflow needs them:

- `list_followed_podcasts` with `days: 30` — the shows the user follows that published recently.
- `list_my_episodes` with `type: "played"`, `pageSize: 50` — real listening behaviour (primary signal).
- `list_my_episodes` with `type: "read"`, `pageSize: 50` — reading/exploration beyond regular listens.

From the results, derive:

- **Shows to Prioritize** — shows with the most played episodes.
- **Shows to Deprioritize** — shows the user follows but that rarely or never appear in their history.
- **Core Interest Areas** — topic clusters inferred from show names, genres and episode titles.

Limitations: `list_followed_podcasts` only returns shows with recent activity, so the follow list is incomplete. Treat the derived context as a good default, not ground truth.

## Ask once per session

Some preferences cannot be inferred from MCP data. Ask for them once per session, in a single message, and only the fields the current workflow needs. Do not re-ask within the same session. Do not persist the answers.

| Field | Needed by |
| --- | --- |
| Output format / length | weekly-recap, episode-notes, topic-research |
| PKM tool (Notion, Obsidian, Logseq, Readwise, none) | weekly-recap, episode-notes |
| Learning language and native language | language-learning |

If the user skips a question, continue with sensible defaults and do not block the workflow.
