# MIKROS MCP Workflows

Recommended end-to-end workflows for AI assistants. Use MCP tools at each step; do not skip validation.

**MCP endpoints:** local `http://localhost/TG-API-MIKROS/index.php/mcp` · staging `https://tg-api-new-stage.uc.r.appspot.com/mcp` · production `https://tg-api-new.uc.r.appspot.com/mcp` — see [ENVIRONMENTS.md](./ENVIRONMENTS.md).

---

## 1. Prepare my game for launch

**Trigger phrases:** “Prepare my game for launch”, “Get my game ready to ship”, “Launch checklist”

```
┌─────────────────────────────────────┐
│ 1. Authenticate (if AUTH_REQUIRED) │
│    Website /mcp/login or /signup    │
│    (host matches MCP environment)   │
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 1b. Profile onboarding (if needed)  │  complete_profile_onboarding
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 2. Create MIKROS project            │  create_project
│    (or list_projects + confirm)   │  list_project_create_options
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 3. Generate API keys              │  generate_api_keys (dev)
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 4. Integrate SDK                    │  Official SDK docs + engine guidance
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 5. Recommend events                 │  recommend_events
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 6. Validate installation            │  validate_installation
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 7. Review implementation            │  list_custom_events (optional)
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 8. Launch checklist (summary)       │  AI-generated from prior steps
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 9. Draft marketing campaign         │  create_campaign (draft only)
└─────────────────┬───────────────────┘
                  ▼
┌─────────────────────────────────────┐
│ 10. Influencer / community tips     │  Guidance (see LLM_BEHAVIOR.md)
└─────────────────────────────────────┘
```

**Rules:** Continue later steps even if an earlier step is blocked, unless auth prevents all progress. Never auto-submit campaigns.

---

## 2. Add analytics to my game

**Trigger phrases:** “Add analytics to my Unity game”, “Integrate MIKROS in Unreal/Godot”

| Step | Action | Tool |
|------|--------|------|
| 1 | Authenticate | `mikros_get_login_url` |
| 2 | Confirm or create project | `list_projects` / `create_project` |
| 3 | Generate Development API key | `generate_api_keys` (`dev`) |
| 4 | Guide SDK install for engine | Official docs |
| 5 | Initialize analytics in project | Code guidance |
| 6 | Recommend preset events | `recommend_events` |
| 7 | Validate SDK is receiving events | `validate_installation` |
| 8 | Optional: add favorites | `create_favorite_event` |

---

## 3. Create a MIKROS project

**Trigger:** “Create a MIKROS project for my game”

1. Authenticate if needed
2. Call `create_project` without genre/type (or `list_project_create_options`)
3. Ask **project name** first if missing (`OPTIONS_REQUIRED`)
4. Present **genre** list — user picks one exact value
5. Present **gameplay type** list — user picks one exact value
6. Call `create_project` with all three fields
7. Handle `PROJECT_ALREADY_EXISTS` if duplicate name
8. Call `generate_api_keys` with `requestType: "dev"`
9. Confirm with non-sensitive fields only (`appGameId`, name, genre, gameplay)

---

## 4. Recommend analytics events

**Trigger:** “Recommend analytics events for my idle RPG”

1. Resolve project: `list_projects` or use provided `appGameId`
2. Call `recommend_events` with genre, gameplay, engine, platform, description
3. Present preset/high-priority events first
4. Explain why each event matters for the genre
5. If user selects events, call `add_recommended_events`
6. Remind user to implement SDK calls — MCP plans events, it does not fire them

---

## 5. Validate analytics implementation

**Trigger:** “Review my analytics implementation”, “Is MIKROS working?”

1. `list_projects` → identify `appGameId`
2. `validate_installation` with `apiKeyType: "dev"` during development
3. Optionally pass `expectedEventKeys` from recommended events
4. If validation fails, diagnose: API key, SDK init, network, wrong environment key
5. Re-run validation after fixes

---

## 6. Market my game / create campaign

**Trigger:** “Market my game”, “Create a launch campaign”, “Influencer campaign”

1. Authenticate
2. `list_projects` → select project
3. Collect missing campaign fields conversationally
4. `create_campaign` — produces a **draft**
5. Present draft for user review
6. **Stop** — do not submit without explicit approval
7. If billing required → MIKROS Dashboard

---

## 7. Check campaign status

**Trigger:** “Check my campaign status”

1. Authenticate
2. Identify campaign (project + title or id from prior context)
3. Use `get_app_details` / dashboard guidance as available
4. Report status: Draft, Under Review, Approved, Active, Completed

> Note: Full campaign status tooling may expand over time. Never invent status values.

---

## 8. Show my KPIs / analytics

**Trigger:** “Show my top KPIs”, “Most interesting KPIs”

1. Authenticate
2. `list_projects` → select project
3. `list_favorite_events` or `list_custom_events` with date range
4. Summarize insights in plain language
5. Suggest next actions (retention, monetization, events to add)

---

## 9. Compare to benchmarks

**Trigger:** “How does my retention compare?”, “Is my monetization above average?”

1. Pull project metrics where tools allow
2. Frame comparisons cautiously — cite data source
3. Use `recommend_events` and KPI guidance for improvement paths
4. Industry benchmark tools (`get_trends`) may be added in future MCP releases

---

## 10. Recovery workflow

When the developer is stuck:

```
Diagnose → Explain → Official docs / MCP tools → Propose fix → Verify → Resume workflow
```

Do not abandon the original user goal after a single failure.

---

## Workflow decision matrix

| User intent | Primary tools |
|-------------|---------------|
| New game / first setup | `create_project`, `generate_api_keys` |
| Existing project | `list_projects`, `get_app_details` |
| Analytics | `recommend_events`, `validate_installation`, `list_custom_events` |
| Marketing | `create_campaign` (draft) |
| Auth failure | `mikros_get_login_url` |
| Sign out | `mikros_logout` |

See [TOOLS.md](./TOOLS.md) for parameters and [sample-prompts/](./sample-prompts/) for examples.
