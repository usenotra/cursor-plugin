---
name: generate-blog
description: Generate a Notra blog post draft about a shipped feature or release. Pass the topic or lookback in the argument.
---

# Generate blog post

## Preflight

- Notra MCP available and a GitHub integration exists.
- If `$ARGUMENTS` names a feature, release, or timeframe, use it as focus. Default lookback `last_14_days`.

## Plan

Generate one `blog_post` draft with a single narrative (not a changelog dump). Keep it unpublished. Confirm the topic if `$ARGUMENTS` is empty and recent activity is broad.

## Commands

1. `list_integrations` + `list_brand_identities`.
2. Optionally `list_posts` with `contentType: changelog` for the same window to see what shipped.
3. `generate_post` with `contentType: "blog_post"`, lookback, `integrations.github`. Pin `selectedItems` if the user named a PR, tag, or commit.
4. Poll `get_post_generation_status`, then `get_post`.

Structure to preserve: hook, problem, solution, impact, next steps. No invented metrics.

## Verification

- Draft `blog_post` with a clear title and narrative
- Status remains `draft`
- Technical claims match GitHub source material

## Summary

```
## Result
- **Action**: generate blog post
- **Status**: success | failed
- **Post ID**:
- **Title**:
- **Focus**:
```

## Next Steps

- `/review-drafts` to tighten or add a migration section
- `/generate-social` for announcement posts
- Copy markdown to the company blog when ready (CMS publish integrations are optional)
