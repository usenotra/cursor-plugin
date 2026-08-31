---
name: generate-social
description: Generate Notra LinkedIn and Twitter draft posts from recent shipped work. Optional argument is platform (linkedin, twitter) or lookback.
---

# Generate social posts

## Preflight

- Notra MCP available and a GitHub integration exists.
- Parse `$ARGUMENTS`: `linkedin` / `linkedin_post`, `twitter` / `twitter_post`, or a lookback window. Default: both platforms, `last_7_days`.

## Plan

Generate social drafts for the requested platforms. One idea per post. Do not publish. LinkedIn ~800 characters; Twitter 280.

## Commands

1. `list_integrations` + `list_brand_identities`.
2. For each requested type, `generate_post` with `contentType` `linkedin_post` or `twitter_post`.
3. Poll each job, then `get_post`.
4. If a job completes with nothing worth posting (empty window), say quality-over-volume and do not invent a post.

Strip hashtags, emoji, PR numbers, and GitHub links from LinkedIn unless the user asked for them.

## Verification

- Drafts exist for the requested types
- Status `draft`
- LinkedIn follows hook → story → takeaway; Twitter is a single compressed update

## Summary

```
## Result
- **Action**: generate social
- **Status**: success | partial | failed
- **LinkedIn**: post id or skipped
- **Twitter**: post id or skipped
```

## Next Steps

- `/review-drafts` to change the hook or add a question
- Post manually to each network after review
