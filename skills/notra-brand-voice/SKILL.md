---
name: notra-brand-voice
description: Create and refine Notra brand identities (tone, audience, custom instructions, website scrape). Use when the user mentions brand voice, tone profile, company description, or on-brand writing in Notra.
---

# Notra brand voice

Brand identity is injected into every generation. Incomplete settings produce generic drafts.

## Read current voice

```
list_brand_identities
get_brand_identity(brandIdentityId)  # use isDefault when unspecified
```

Report name, company, tone, audience, language, custom instructions, and whether it is default.

## Generate from a website

```
generate_brand_identity(websiteUrl, name?)
get_brand_identity_generation_status(jobId)  # 30–60s typical
get_brand_identity(id)                       # review, then refine
```

Confirm the extracted company name, description, tone, and audience with the user before treating them as final.

## Update settings

`update_brand_identity` accepts partial fields:

| Field | Notes |
| --- | --- |
| `name` | 1–120 chars |
| `websiteUrl` | Homepage or about page |
| `companyName` | Product/company name |
| `companyDescription` | 10–4000 chars; 2–4 sentences |
| `toneProfile` | `Conversational` \| `Professional` \| `Casual` \| `Formal` |
| `customTone` | Freeform override, max 1000 |
| `customInstructions` | Max 4000. Style rules, not content topics |
| `audience` | 10–1000 chars. Be specific |
| `language` | Content language enum from the tool schema |
| `isDefault` | Only `true` is valid; makes this the org default |

Do not delete the default identity. `delete_brand_identity` disables automations that reference it — confirm first.

## Tone presets

| Profile | When |
| --- | --- |
| Conversational | Dev tools, startups, OSS. Warm, we/you, founder-to-community |
| Professional | B2B / enterprise. Clear, confident, business value |
| Casual | Consumer / community. Friendly; emojis only if requested |
| Formal | Finance, health, compliance. Precise, complete sentences |

## Audience

- Developers: PR links, technical terms, `@author`, implementation detail
- Customers / stakeholders: outcomes, no PR numbers, plain language

Write audience as a role + what they care about, e.g. `Senior engineers at B2B SaaS teams who want concrete migration guidance`.

## Custom instructions

Good: `Always include PR links. Say "shipped" not "delivered". Mention security and performance when present.`

Bad: `Use simple language` (too vague) or instructions that contradict the tone profile.

After a voice change, generate a test changelog (`last_7_days`) so the user can judge the new voice. Existing drafts are not rewritten.
