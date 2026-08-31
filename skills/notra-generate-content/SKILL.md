---
name: notra-generate-content
description: Generate, list, edit, and publish Notra posts (changelogs, blog posts, LinkedIn, Twitter, images) from GitHub and Linear activity. Use when the user asks to write a changelog, launch post, social update, or review Notra drafts.
---

# Generate Notra content

MCP-first. Read [tools.md](../notra/tools.md) for argument details.

## Workflow

Copy and track:

```
- [ ] Confirm content type and lookback
- [ ] Resolve GitHub/Linear integrations
- [ ] Resolve brand identity
- [ ] Queue generate_post
- [ ] Poll until completed
- [ ] Open the draft with get_post
- [ ] Edit only if asked; keep status draft
```

### 1. Content type

| User intent | `contentType` |
| --- | --- |
| Changelog, release notes, weekly update | `changelog` |
| Long-form story, launch article | `blog_post` |
| LinkedIn | `linkedin_post` |
| Twitter/X | `twitter_post` |
| Image / visual | `image` |

If unspecified, default to `changelog` for "what shipped" and `blog_post` for a named feature.

### 2. Lookback

| Cadence | `lookbackWindow` |
| --- | --- |
| Today | `current_day` |
| Yesterday recap | `yesterday` |
| Weekly (default) | `last_7_days` |
| Two weeks | `last_14_days` |
| Monthly | `last_30_days` |

### 3. Sources

```
list_integrations → pick GitHub (and Linear if needed)
list_brand_identities → use default unless the user names one
```

Pass `integrations.github: [<id>, ...]` (and `integrations.linear` when relevant). Do not pass GitHub PATs into generation.

Optional `dataPoints`: `includePullRequests`, `includeCommits`, `includeReleases` (default true), `includeLinearData` (default false).

Optional `selectedItems` to pin specific PRs, commits, tags, or Linear issues.

### 4. Generate and poll

```
generate_post → job.id
get_post_generation_status(jobId) until status is completed | failed | skipped
```

Typical runtime is 15–45s. If `failed`, report `error` and stop. Rate limit on generate is 10 requests / minute / org.

### 5. Review

```
get_post(postId)  # from job.postId
```

Present title, type, status, and markdown. Suggest edits; do not publish unless asked.

To edit: `update_post` with `markdown` and/or `title`. To publish: `status: "published"` after confirmation. To remove: `delete_post` after confirmation.

## Content-type rules

**Changelog** — Summary 120–180 words, then Highlights (max 5: security > breaking > features > reliability), then More Updates by category. Include PR links and `@author` when the audience is developers. Filter internal chores.

**Blog post** — One narrative (problem → solution → impact). Do not dump every PR. 1000–3000 words is expected.

**LinkedIn** — One idea. Hook → story → lesson → takeaway. ~800 characters. Short sentences. No hashtags, emojis, PR numbers, or GitHub links unless the user asks.

**Twitter** — One 280-character update. Same story as LinkedIn, compressed.

Never invent PR numbers, metrics, or quotes. If GitHub data is thin, say so and ask for a longer lookback or better PR titles.

## List drafts

```
list_posts(status: "draft", contentType: "<type>", limit: 10, sort: "desc")
```

## CLI fallback

```bash
notra posts generate --content-type changelog --lookback last_7_days --wait
notra posts list --status draft --limit 10
notra posts get <id> --markdown
notra posts update <id> --status published
```
