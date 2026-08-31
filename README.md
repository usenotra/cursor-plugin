# Notra Cursor plugin

[Notra](https://www.usenotra.com) turns shipped work — merged PRs, releases, commits, Linear issues — into draft changelogs, blog posts, and social updates in your brand voice.

This plugin packages the [hosted Notra MCP server](https://docs.usenotra.com/devtools/mcp), Cursor skills, slash commands, a content agent, and a rule so an agent can operate a Notra workspace from Cursor.

## What you get

| Component | Location | Purpose |
| --- | --- | --- |
| MCP | `mcp.json` | Hosted server at `https://mcp.usenotra.com/mcp` |
| Skills | `skills/` | Workflows for generate, brand, writing skills, workspace setup, GEO |
| Commands | `commands/` | `/setup-notra`, `/generate-changelog`, `/generate-blog`, `/generate-social`, `/review-drafts`, `/manage-skills` |
| Agent | `agents/notra-content.md` | Content operator that keeps drafts unpublished until you ask |
| Rule | `rules/notra.mdc` | MCP-first, confirm destructive/billed actions |

## Slash commands

Type these in Agent chat (do not pick **Create … skill**):

| Slash | What it does |
| --- | --- |
| `/setup-notra` | Connect GitHub + brand voice, generate a test changelog draft |
| `/generate-changelog` | Draft a changelog from recent GitHub activity |
| `/generate-blog` | Draft a launch/blog post |
| `/generate-social` | Draft LinkedIn and Twitter posts |
| `/review-drafts` | List and edit drafts (publish only if you ask) |
| `/manage-skills` | Create/update Notra org writing skills such as `humanizer` |

If `/setup-notra` offers **Create /setup-notra skill**, the plugin is not loaded in that chat. Enable it in **Customize**, then **Developer: Reload Window**. Use the sidebar Agent chat in this repo, not a Cloud / New Agent that cannot see `~/.cursor/plugins/local`.

## Install

### Local (development)

```bash
mkdir -p ~/.cursor/plugins/local
ln -s /absolute/path/to/this-repo ~/.cursor/plugins/local/notra
```

Reload the window (**Developer: Reload Window**), then open **Customize** and confirm the Notra plugin, MCP server, and skills are enabled.

On Teams/Enterprise, **Allow Local Plugin Imports** must be on (Dashboard → Settings → Security & Identity → Marketplace and Plugins).

### Marketplace

When published, install from **Customize** and set the API key under **Plugins → Configure**.

## Configure

1. Create a key at [app.usenotra.com](https://app.usenotra.com) → **Developer → API Keys**.
2. Grant read and write for posts, brand identities, integrations, schedules, chats, and skills.
3. Set `NOTRA_API_KEY` on the plugin (Customize → Notra → Configure). Do not commit the key.

The hosted MCP also supports [OAuth discovery](https://mcp.usenotra.com/.well-known/oauth-protected-resource). This plugin uses a bearer API key so team marketplaces can inject the secret via plugin variables.

Optional CLI (same key):

```bash
npm i -g notra
notra init --api-key ntra_...
```

## Skills

| Skill | When to use |
| --- | --- |
| `setup-notra` | `/setup-notra` — first-time connect |
| `generate-changelog` | `/generate-changelog` |
| `generate-blog` | `/generate-blog` |
| `generate-social` | `/generate-social` |
| `review-drafts` | `/review-drafts` |
| `manage-skills` | `/manage-skills` for Notra org writing skills |
| `notra` | Router: product overview, auth, which skill to load |
| `notra-generate-content` | Changelogs, blogs, LinkedIn, Twitter, images |
| `notra-brand-voice` | Tone, audience, website scrape |
| `notra-writing-skills` | Org writing skills such as `humanizer` |
| `notra-workspace` | GitHub connect, cron schedules, chats |
| `notra-geo` | GEO visibility, competitors, briefs, AI traffic |

Notra **writing skills** (`list_skills`, `create_skill`, …) live in your Notra org and run during generation. They are separate from these Cursor skills.

## MCP tools

The server is [`@usenotra/mcp`](https://github.com/usenotra/notra-mcp). Cursor may show names with or without a `notra_` prefix.

Coverage includes posts, brand identities, GitHub integrations, schedules, chats, writing skills, GEO (plan-gated), and `submit_feedback`.

GEO scans, sequence runs, and content briefs use billed AI credits — the agent asks before starting them.

## Typical flow

1. `/setup-notra` — connect GitHub and brand voice, generate a test changelog.
2. `/generate-changelog` — weekly draft from `last_7_days`.
3. `/review-drafts` — edit markdown; publish only when you say so.
4. Optional: `/generate-blog` or `/generate-social`.

Event-based release/push triggers are configured in the Notra dashboard (Automation → Events). Cron schedules can be created through MCP.

## Docs

- Product: [docs.usenotra.com](https://docs.usenotra.com)
- Docs index: [llms.txt](https://docs.usenotra.com/llms.txt)
- MCP: [devtools/mcp](https://docs.usenotra.com/devtools/mcp)
- CLI: [devtools/cli](https://docs.usenotra.com/devtools/cli)
- Cursor plugins: [cursor.com/docs/plugins](https://cursor.com/docs/plugins)

Logo from [Notra brand guidelines](https://www.usenotra.com/brand).
