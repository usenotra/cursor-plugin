# Notra MCP tools

Source of truth: [`@usenotra/mcp`](https://github.com/usenotra/notra-mcp) (`createServer` in `src/server.ts`). Hosted endpoint: `https://mcp.usenotra.com/mcp`.

Some Cursor listings prefix names with `notra_`. Treat `list_posts` and `notra_list_posts` as the same tool.

## Posts

| Tool | Use |
| --- | --- |
| `list_posts` | Filter with `sort`, `limit`, `page`, `status`, `contentType`, `brandIdentityId` |
| `get_post` | Full HTML + markdown by `postId` |
| `update_post` | `title`, `slug`, `markdown`, `status` (`draft` \| `published`) |
| `delete_post` | Destructive. Confirm first |
| `generate_post` | Queue generation. Returns `job.id` |
| `get_post_generation_status` | Poll until `completed` / `failed` / `skipped` |

Generatable `contentType`: `changelog`, `blog_post`, `linkedin_post`, `twitter_post`, `image`.

Also stored (list/get only): `investor_update`.

`lookbackWindow`: `current_day`, `yesterday`, `last_7_days`, `last_14_days`, `last_30_days`.

Prefer `integrations.github` IDs from `list_integrations` over deprecated `repositoryIds`.

## Brand identities

| Tool | Use |
| --- | --- |
| `list_brand_identities` | All identities |
| `get_brand_identity` | Tone, audience, language, instructions |
| `update_brand_identity` | Partial update. `toneProfile`: `Conversational`, `Professional`, `Casual`, `Formal`. `isDefault: true` makes it the org default |
| `delete_brand_identity` | Cannot delete the default. Disables automations that reference it |
| `generate_brand_identity` | Scrape `websiteUrl` |
| `get_brand_identity_generation_status` | Poll scrape job |

## Integrations

| Tool | Use |
| --- | --- |
| `list_integrations` | GitHub, Linear, Slack |
| `create_github_integration` | `owner`, `repo`, optional `branch`, optional `token` for private repos |
| `delete_integration` | GitHub or Linear. Disables related automations |

## Schedules

| Tool | Use |
| --- | --- |
| `list_schedules` | Optional `repositoryIds` filter |
| `create_schedule` | Cron `daily` / `weekly` / `monthly` in UTC |
| `update_schedule` | PATCH-style replace of fields |
| `delete_schedule` | Destructive |

`sourceType` is `cron`. Weekly needs `dayOfWeek` (0=Sunday). Monthly needs `dayOfMonth` (1–31). Times are UTC.

## Chats

| Tool | Use |
| --- | --- |
| `list_chats` | Sessions |
| `get_chat` | Messages |
| `get_chat_by_external_channel` | Discord or Slack channel ID |
| `create_chat` | Start a chat; streamed reply |
| `post_chat_message` | Continue a chat |

## Writing skills (workspace)

These are Notra org skills used during generation (for example `humanizer`), not Cursor plugin skills.

| Tool | Use |
| --- | --- |
| `list_skills` | Summaries (`name`, `description`, `isSystem`) |
| `get_skill` | Full Markdown `content` by name |
| `create_skill` | `name`, `description`, `content` |
| `update_skill` | `currentName` plus optional `name` / `description` / `content` |
| `delete_skill` | By name. Confirm. Do not delete system skills unless asked |

Name pattern: `^[a-z0-9][a-z0-9-]*[a-z0-9]$|^[a-z0-9]$` (max 64).

## GEO (plan + org-scoped key)

Call `list_projects` first. Confirm before billed scans, sequence runs, and briefs.

Projects: `list_projects`, `get_project`, `create_project`, `update_project`, `delete_project`

Settings: `get_geo_settings`, `update_geo_settings`

Prompts: `list_geo_prompts`, `create_geo_prompt`, `update_geo_prompt`, `delete_geo_prompt`, `import_geo_prompts`

Sequences: `list_geo_sequences`, `create_geo_sequence`, `update_geo_sequence`, `delete_geo_sequence`, `run_geo_sequence`

Competitors: `list_geo_competitors`, `upsert_geo_competitor`, `suggest_geo_competitors`, `delete_geo_competitor`, `import_geo_competitors`

Scans / visibility: `create_geo_scan`, `list_geo_scans`, `get_geo_scan`, `get_geo_visibility_overview`, `get_geo_visibility_timeseries`, `get_geo_prompt_results`, `get_geo_competitor_share`, `get_geo_language_share`, `get_geo_competitor_detail`

Briefs: `list_geo_content_gaps`, `list_geo_content_briefs`, `plan_geo_content_brief`, `get_geo_content_brief`, `approve_geo_content_brief`

Readiness: `get_geo_agent_readiness`, `start_geo_agent_readiness_scan`

Traffic: `get_geo_traffic_overview`, `get_geo_traffic_log`, `list_geo_traffic_journeys`, `get_geo_traffic_journey`, `list_geo_traffic_pages`, `get_geo_ingest_setup`, `issue_geo_ingest_token`, `rotate_geo_ingest_token`

## Feedback

`submit_feedback` — bugs, features, questions, praise. No extra auth. Include steps, error text, and URLs.
