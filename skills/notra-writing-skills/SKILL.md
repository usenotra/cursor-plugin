---
name: notra-writing-skills
description: Manage Notra workspace writing skills (list, get, create, update, delete), including the humanizer skill used during content generation. Use when the user wants reusable writing rules in Notra, not Cursor skills.
---

# Notra writing skills

These are **organization skills stored in Notra**, loaded during generation (for example to humanize a near-final changelog). They are not Cursor plugin skills.

## When to use which

| Store | What it is |
| --- | --- |
| Notra `list_skills` / `create_skill` | Writing instructions Notra applies when generating posts |
| This Cursor plugin's `skills/` | How the agent operates Notra |

If the user says "add a humanizer" or "save this voice as a skill in Notra", use the MCP skill tools.

## List and read

```
list_skills
get_skill(name)   # full Markdown content
```

`isSystem: true` skills are built-in. Do not delete or overwrite them unless the user explicitly asks.

The built-in example name is `humanizer`: polish near-final drafts so they sound natural and specific.

## Create

Required:

- `name`: lowercase, digits, hyphens; `^[a-z0-9][a-z0-9-]*[a-z0-9]$|^[a-z0-9]$`; max 64
- `description`: when to apply it (max 1000). Third person, include triggers
- `content`: full Markdown instructions (max 200000)

Keep `content` concise. Notra injects it during generation.

409 means the name exists — `get_skill` then `update_skill` instead.

## Update

```
update_skill(currentName, name?, description?, content?)
```

Send only changed fields. Confirm before renaming or replacing a system skill.

## Delete

```
delete_skill(name)
```

Confirm. Prefer disabling via content/description edits over delete when unsure.

## Authoring a good Notra skill

Mirror Agent Skill practice:

1. Description = what + when
2. Instructions the generator can follow without extra context
3. Concrete do/don't examples
4. No time-sensitive "before date X" rules

Humanizer pattern:

```markdown
Polish near-final copy. Keep facts, PR numbers, and names unchanged.
Replace filler and corporate phrasing with specific verbs.
Do not add claims, metrics, or links that are not already in the draft.
```
