---
name: manage-skills
description: List, create, or update reusable writing skills in the Notra workspace (for example humanizer).
---

# Manage Notra writing skills

## Preflight

- Notra MCP available (`list_skills` succeeds).
- `$ARGUMENTS` may be a skill name or an instruction such as "create humanizer".

These are Notra org skills, not Cursor plugin skills.

## Plan

Read existing skills first. Create or update only what the user asked. Confirm before deleting or overwriting `isSystem` skills.

## Commands

1. `list_skills`. If a name is given, `get_skill(name)`.
2. Create: `create_skill` with kebab-case `name`, trigger `description`, and Markdown `content`.
3. Update: `update_skill` with `currentName` and changed fields only.
4. Delete: confirm, then `delete_skill`. Skip system skills unless explicitly requested.

Name pattern: lowercase letters, digits, hyphens; max 64.

## Verification

- `get_skill` returns the new description and content
- `list_skills` includes the name
- System skills unchanged unless the user asked

## Summary

```
## Result
- **Action**: list | create | update | delete
- **Status**: success | failed
- **Skill**: name
- **System**: true | false
```

## Next Steps

- Generate a test changelog to see the skill applied
- Refine brand `customInstructions` if the rule belongs on every post, not a named skill
