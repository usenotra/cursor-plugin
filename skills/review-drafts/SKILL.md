---
name: review-drafts
description: List Notra draft posts, show markdown, and apply requested edits without publishing unless asked. Use when the user runs /review-drafts or wants to edit Notra drafts.
---

# /review-drafts

## Preflight

- Notra MCP available.
- `$ARGUMENTS` may be a post ID, content type, or edit instruction.

## Plan

List recent drafts (or open the given ID), summarize them, and apply only the edits the user requested. Do not publish or delete without an explicit yes.

## Commands

1. If no ID: `list_posts` with `status: "draft"`, `sort: "desc"`, `limit: 10`. Filter `contentType` if specified.
2. `get_post` for the selected id(s).
3. Show title, type, status, createdAt, and markdown.
4. On edit requests: `update_post` with new `markdown` / `title` only.
5. On explicit publish: `update_post` with `status: "published"`.
6. On explicit delete: `delete_post` after confirmation.

Keep brand voice from `get_brand_identity`. Do not add facts that are not in the draft or GitHub metadata.

## Verification

- Listed posts match `draft` unless the user asked for published
- After update, `get_post` reflects the new markdown/title/status

## Summary

```
## Result
- **Action**: review drafts
- **Status**: success | partial | failed
- **Posts touched**: ids and titles
- **Published**: yes | no
```

## Next Steps

- Generate a missing type with `/generate-changelog`, `/generate-blog`, or `/generate-social`
- Adjust voice with the `notra-brand-voice` skill if drafts sound off-brand
