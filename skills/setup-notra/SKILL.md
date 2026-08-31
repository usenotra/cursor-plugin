---
name: setup-notra
description: Connect Notra MCP, GitHub, and brand voice, then generate a test changelog draft. Use when the user runs /setup-notra or asks to set up Notra.
---

# /setup-notra

Connect this workspace to Notra, then leave one changelog as a draft.

## Preflight

- MCP **notra** server connected. If auth fails, tell the user to set `NOTRA_API_KEY` from Developer → API Keys (Customize → Notra → Configure) and reload.
- Do not print the key.
- Note the current git remote `owner/repo` if this workspace is a GitHub project — offer it as the default integration.

## Plan

State that you will: verify the API key, connect GitHub if missing, ensure a brand identity exists, then generate one `changelog` draft for `last_7_days` without publishing.

## Commands

1. `list_integrations` and `list_brand_identities`.
2. If no GitHub integration: ask for `owner/repo` (and a PAT only for private repos). Then `create_github_integration`.
3. If no brand identity: ask for a website URL and run `generate_brand_identity` + poll `get_brand_identity_generation_status`. Let the user confirm tone/audience, then `update_brand_identity` if needed.
4. `generate_post` with `contentType: changelog`, `lookbackWindow: last_7_days`, `integrations.github` IDs.
5. Poll `get_post_generation_status`, then `get_post`.

Keep status `draft`.

## Verification

- At least one GitHub integration listed
- At least one brand identity, with a default
- Test post exists as `draft`

## Summary

```
## Result
- **Action**: workspace setup
- **Status**: success | partial | failed
- **GitHub**: owner/repo or missing
- **Brand**: name / tone / default
- **Test post**: id, title, status
```

## Next Steps

- `/generate-changelog` on a schedule
- `/generate-blog` for a named feature
- `/review-drafts` to edit copy
- Dashboard event triggers for GitHub releases: Automation → Events
