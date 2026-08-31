---
name: notra-workspace
description: Set up a Notra workspace: API key, GitHub integrations, schedules, and chats. Use for first-time setup, connecting repos, cron automations, or Notra chat sessions.
---

# Notra workspace

## First-time setup

```
- [ ] API key works (list_integrations or list_brand_identities)
- [ ] GitHub repo connected
- [ ] Brand identity exists (scrape or manual)
- [ ] Optional weekly changelog schedule
- [ ] Generate one test changelog and leave it draft
```

Keys: [app.usenotra.com](https://app.usenotra.com) → Developer → API Keys. Grant posts, brand identities, integrations, schedules, chats, and skills.

Private GitHub repos need a classic PAT with `repo` scope. Public repos do not. Never print the PAT.

## GitHub

```
create_github_integration(owner, repo, branch?, token?)
list_integrations
```

Notra reads PR titles/descriptions, labels, releases, default-branch commits, and repo metadata. It does not write to GitHub.

IDs from `list_integrations` are what `generate_post` and `create_schedule` need.

`delete_integration` disables related schedules/events — confirm first.

Linear: list/delete via MCP; create Linear from the dashboard. Slack is on the roadmap.

## Schedules (UTC)

```
create_schedule
  name
  sourceType: "cron"
  sourceConfig.cron: { frequency, hour, minute, dayOfWeek?, dayOfMonth? }
  targets.repositoryIds: [...]   # integration/repo IDs from list_integrations
  outputType: changelog | blog_post | linkedin_post | twitter_post | image
  enabled: true
  autoPublish: false             # unless the user wants auto-publish
  lookbackWindow: last_7_days
```

| Frequency | Extra field |
| --- | --- |
| `daily` | hour, minute |
| `weekly` | `dayOfWeek` 0=Sunday … 6=Saturday |
| `monthly` | `dayOfMonth` 1–31 |

Leave `autoPublish` false so drafts can be reviewed.

Example: Friday 17:00 UTC weekly changelog, last 7 days.

Event-based release/push triggers are configured in the dashboard (Automation → Events). MCP covers cron schedules, not webhook trigger CRUD.

## Chats

Use `create_chat` / `post_chat_message` to refine copy inside Notra's editor chat. `get_chat_by_external_channel` looks up Discord/Slack threads.

## CLI fallback

```bash
notra auth login
notra integrations github --owner acme --repo app
notra brands generate --website-url https://acme.com --wait
notra schedules create --name "Weekly changelog" --frequency weekly --hour 17 --minute 0 --day-of-week 5 --output-type changelog --lookback last_7_days --enabled
```

Install: `npm i -g notra` or `npx notra`.
