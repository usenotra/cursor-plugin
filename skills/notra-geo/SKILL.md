---
name: notra-geo
description: Operate Notra GEO (generative engine optimization): projects, tracked prompts, competitors, visibility scans, content briefs, agent readiness, and AI traffic. Use when the user mentions GEO, AI search visibility, content gaps, or AI crawler traffic in Notra.
---

# Notra GEO

GEO tools need an **organization-scoped API key** and the **GEO plan**. If a call fails with plan/permission errors, say so and stop retrying.

Always `list_projects` first. Most tools take `projectId`.

## Safety

Confirm with the user before:

- `create_geo_scan`
- `run_geo_sequence` (can take minutes, billed)
- `plan_geo_content_brief` (billed, can take minutes)
- `approve_geo_content_brief` (starts the article writer)
- `start_geo_agent_readiness_scan`
- `rotate_geo_ingest_token` (invalidates existing tokens)
- `delete_project` (cascades GEO data)

## Typical flows

### See where the brand is mentioned

```
list_projects → get_geo_settings
get_geo_visibility_overview
get_geo_visibility_timeseries
get_geo_prompt_results
get_geo_competitor_share
```

### Track a new prompt or competitor

```
create_geo_prompt / import_geo_prompts
upsert_geo_competitor / suggest_geo_competitors / import_geo_competitors
```

Confirm before `update_geo_settings` — it replaces the settings document and restarts the scan cycle.

### Run a scan

```
create_geo_scan → list_geo_scans / get_geo_scan
```

Uses AI credits.

### Close a content gap

```
list_geo_content_gaps          # prompts where competitors appear and this brand does not
plan_geo_content_brief         # research + plan (billed)
get_geo_content_brief
approve_geo_content_brief      # only after the user accepts the brief
```

### AI traffic

```
get_geo_ingest_setup           # install snippets
issue_geo_ingest_token         # first-time
get_geo_traffic_overview
get_geo_traffic_log
list_geo_traffic_pages
list_geo_traffic_journeys / get_geo_traffic_journey
```

Rotate tokens only when asked.

### Agent readiness

```
get_geo_agent_readiness
start_geo_agent_readiness_scan   # confirm first
```

Tool map: [tools.md](../notra/tools.md)
