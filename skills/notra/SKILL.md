---
name: notra
description: Routes Notra work to the right skill and MCP tools. Use when the user mentions Notra, usenotra, changelogs from GitHub, brand voice, writing skills, schedules, chats, GEO visibility, or content generation from shipped work.
---

# Notra

Notra turns shipped work (merged PRs, releases, commits, Linear issues) into draft changelogs, blog posts, LinkedIn/Twitter posts, and images in the workspace brand voice.

Official docs: [docs.usenotra.com](https://docs.usenotra.com) — start at the index: https://docs.usenotra.com/llms.txt

Dashboard: [app.usenotra.com](https://app.usenotra.com)

## MCP first

Use the connected **notra** MCP server. Do not invent REST payloads when a tool exists.

Tool names from [`@usenotra/mcp`](https://github.com/usenotra/notra-mcp) are unprefixed (`list_posts`, `generate_post`). Some clients list them as `notra_list_posts`. Use whichever names the connected server exposes.

If MCP is missing or unauthorized:

1. Confirm the plugin is installed and `NOTRA_API_KEY` is set (Developer → API Keys).
2. Grant the key read/write for posts, brand identities, integrations, schedules, chats, and skills.
3. Reload the window. Fallback REST is `https://api.usenotra.com` with `Authorization: Bearer <key>`.

## Choose a skill

| Task | Skill |
| --- | --- |
| `/setup-notra` first-time connect | `setup-notra` |
| `/generate-changelog` | `generate-changelog` |
| `/generate-blog` | `generate-blog` |
| `/generate-social` | `generate-social` |
| `/review-drafts` | `review-drafts` |
| `/manage-skills` Notra org writing skills | `manage-skills` |
| Generate, list, edit, or publish posts | `notra-generate-content` |
| Brand identity, tone, audience, website scrape | `notra-brand-voice` |
| Workspace writing skills (`humanizer` and custom skills) | `notra-writing-skills` |
| Connect GitHub, schedules, chats, first-time setup | `notra-workspace` |
| GEO visibility, competitors, content briefs, AI traffic | `notra-geo` |

## Safety

- Confirm before `delete_*`, publishing (`status: published`), GEO scans, sequence runs, and content briefs (those use billed credits).
- Do not echo API keys or GitHub PATs.
- Leave new posts as `draft` unless the user asks to publish.
- `submit_feedback` reports bugs/requests to the Notra team. Use it when a tool fails or a requested feature is missing.

## Docs

Prefer current docs over training data. Fetch https://docs.usenotra.com/llms.txt, then the matching `.md` page.

Key pages:

- [How it works](https://docs.usenotra.com/concepts/how-it-works.md)
- [MCP](https://docs.usenotra.com/devtools/mcp.md)
- [CLI](https://docs.usenotra.com/devtools/cli.md)
- [Brand voice](https://docs.usenotra.com/concepts/brand-voice.md)

Full MCP tool map: [tools.md](tools.md)
