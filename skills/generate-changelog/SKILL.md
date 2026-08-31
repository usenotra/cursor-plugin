---
name: generate-changelog
description: Generate a Notra changelog draft from recent GitHub activity. Use when the user runs /generate-changelog or asks for release notes from shipped PRs.
---

# /generate-changelog

## Preflight

- Notra MCP available (`list_integrations` succeeds).
- At least one GitHub integration. If none, run `/setup-notra` instead.
- `$ARGUMENTS` may be a lookback: `current_day`, `yesterday`, `last_7_days` (default), `last_14_days`, `last_30_days`.

## Plan

Generate a `changelog` draft. Do not publish. Name the integrations, brand identity, and lookback you will use.

## Commands

1. `list_integrations` — GitHub IDs (Linear only if the user wants issue context).
2. `list_brand_identities` — default unless specified.
3. `generate_post` with `contentType: "changelog"`, chosen `lookbackWindow`, `integrations.github`.
4. Poll `get_post_generation_status` until `completed` / `failed` / `skipped`.
5. `get_post` and show markdown.

If generation fails because PRs are empty or undescriptive, say so. Do not invent entries.

## Verification

- Job status `completed` and `postId` set
- Post `contentType` is `changelog` and `status` is `draft`
- Highlights are high-signal; no fabricated PR numbers

## Summary

```
## Result
- **Action**: generate changelog
- **Status**: success | failed
- **Post ID**:
- **Title**:
- **Lookback**:
- **Sources**:
```

## Next Steps

- Edit with `/review-drafts`
- Publish only if the user asks (`update_post` `status: published`)
- `/generate-social` for a LinkedIn/Twitter cut of the same window
