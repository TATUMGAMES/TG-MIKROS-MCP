# MIKROS MCP Tools

Catalog of tools exposed by MIKROS MCP. AI assistants should call these instead of guessing API requests.

## Transport

MIKROS MCP is served by the **MIKROS API** over HTTP:

```
POST {MIKROS_API_BASE_URL}/mcp
```

| Environment | MCP endpoint |
|-------------|--------------|
| **Local** | `http://localhost/TG-API-MIKROS/index.php/mcp` |
| **Staging** | `https://tg-api-new-stage.uc.r.appspot.com/mcp` |
| **Production** | `https://tg-api-new.uc.r.appspot.com/mcp` |

See [ENVIRONMENTS.md](./ENVIRONMENTS.md) for website auth URLs and health checks.

JSON-RPC methods: `initialize`, `notifications/initialized`, `tools/list`, `tools/call`

Auth headers: `Authorization: Bearer <token>`, `Mcp-Session-Id: <mcpSessionId>`

Tool registry: `TG-API-MIKROS/application/libraries/McpToolRegistry.php`

See [API_REFERENCE.md](./API_REFERENCE.md) for endpoints and response codes.

---

## Authentication tools

### `mikros_get_login_url`

Get a browser authorization URL so the user can grant MCP access.

| | |
|---|---|
| **Auth required** | No |
| **When to use** | First tool call fails with `AUTH_REQUIRED`, or user needs to connect |

**Returns:** `loginUrl`, `signupUrl` (via website), `mcpSessionId` after completion, instructions to retry.

**AI behavior:** Present connect and create-account paths. Do not ask for password in chat unless browser flow is impossible.

---

### `mikros_signin`

Sign in with email and password. **Fallback only** — prefer browser authorization.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `email` | Yes | MIKROS account email |
| `password` | Yes | Account password |

---

### `mikros_signup`

Create a new MIKROS account and send verification email. Does not sign into the website dashboard.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `email` | Yes | |
| `password` | Yes | Min 6 characters |
| `confirmPassword` | Yes | Must match password |

---

### `mikros_logout`

Sign out and clear MCP session token.

| | |
|---|---|
| **Auth required** | Yes |

---

### `complete_profile_onboarding`

Save required profile fields after MCP signup: full name, company name, company size.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `fullName` | When missing | Ask first |
| `companyName` | When missing | Ask after full name |
| `companySize` | When missing | Ask after company name |

---

## Project tools

### `list_project_create_options`

Fetch allowed **game genre** and **gameplay type** options for project creation.

| | |
|---|---|
| **Auth required** | Yes |

**AI behavior:** Call before `create_project` when genre/gameplay are unknown. Present full option lists; never invent values.

---

### `create_project`

Create a MIKROS project/app with genre and gameplay type.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `accessToken` / `mcpSessionId` | Yes | Auth |
| `appName` | Yes (final call) | Display name |
| `gameGenre` | Yes (final call) | Id or exact name from options |
| `gameplayType` | Yes (final call) | Id or exact name from options |
| `userTimeZone` | No | IANA timezone, default UTC |

**Response codes:** `OPTIONS_REQUIRED`, `PROJECT_ALREADY_EXISTS`, `PROJECT_COMPLETED`

**After success:** call `generate_api_keys` with `requestType: "dev"`. Keys are never printed in chat.

**Example prompt:** “Create a MIKROS project for my Unity game.”

---

### `list_projects`

List approved MIKROS projects.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` | No | Filter to one project |

---

### `get_app_details`

Get details for a single project.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` | Yes | Project identifier |

---

### `delete_project` / `delete_app`

Archive/delete a project after explicit user confirmation.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` or `appName` | Yes | Target project |
| `confirmProjectName` | Yes | Exact name confirmation |

**Response codes:** `DELETE_PROJECT_CONFIRMATION_REQUIRED`, `DELETE_PROJECT_DETAILS_REQUIRED`

**AI behavior:** User must reply with exact confirmation (e.g. `delete {projectName}`) before delete proceeds.

---

## API key tools

### `generate_api_keys`

Generate or refresh analytics API keys.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` | Yes | Project id |
| `requestType` | Yes | `dev`, `QA`, or `prod` |

**AI behavior:**

- Generate **Development** key after project creation
- Never print full Production keys in responses
- Mask keys when showing confirmation (e.g. `****mM96`)

---

## Analytics & events tools

### `validate_installation`

Check whether the SDK appears correctly integrated (project setup, API keys, recent event ingestion).

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` or `appName` | Yes | Target project |
| `apiKeyType` | No | `dev`, `QA`, `prod` — **defaults to `prod`** in API; AI should pass `dev` during development |
| `lookbackHours` | No | Default 24, max 720 |
| `expectedEventKeys` | No | e.g. `["mikros_app_open","session_start"]` |

**Returns:** `INSTALLATION_VALIDATION` with checks (project found, API key exists, SDK events received, recent events, metadata) and confidence level.

**AI behavior:** Run before declaring integration complete. Use `apiKeyType: "dev"` while building.

---

### `recommend_events`

Genre-aware instrumentation strategy from the Event Knowledge Library.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` / `appName` | No | Infer genre from project |
| `genre` | No | e.g. RPG, Puzzle, Strategy |
| `gameplayType` | No | e.g. Casual, Competitive |
| `engine` | No | Unity, Unreal, etc. |
| `platform` | No | iOS, Android, Steam, Web |
| `gameDescription` | No | Free-text context |

**Example prompt:** “Recommend analytics events for my idle RPG.”

---

### `add_recommended_events`

Add selected recommendations to a project instrumentation plan (does not fire live events).

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` or `appName` | Yes | |
| `eventNumbers` | No | From recommendation list, e.g. `[1,2,3]` |
| `eventNames` | No | Alternative to numbers |

---

### `list_custom_events`

List custom events for a project.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` or `appName` | Yes | |
| `apiKeyType` | No | `dev`, `QA`, `prod` |
| `startDate` / `endDate` | No | Date filters |

---

### `list_favorite_events`

List favorite custom-event charts.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` | Yes | |
| `apiKeyType` | No | |
| `startDate` / `endDate` | No | `YYYY-MM-DD` |

---

### `create_favorite_event`

Create a favorite chart for a custom event.

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` or `appName` | Yes | |
| `eventKey` | Yes | Custom event key |
| `chartType` | No | `lineChart`, `barChart`, etc. |
| `apiKeyType` | No | |

---

## Marketing tools

### `create_campaign`

Create a marketing campaign for an existing project (submitted for **review**).

| Parameter | Required | Description |
|-----------|----------|-------------|
| `appGameId` or `appName` | Yes | Existing project |
| `title` | When missing | Campaign title |
| `businessInfo` | When missing | Overview |
| `dailyBudget` | When missing | |
| `durationInDays` | When missing | Often 30, 60, 90 |
| `location` | Yes | Target region |
| `appLogoUrl` | Yes | Public URL (MCP does not upload files) |
| `screenshotUrls` | Yes | At least one public URL |
| `featureVideoUrls` | No | Public URLs |
| Store / social URLs | No | Apple, Google, Steam, social links |

**Response code:** `CAMPAIGN_DETAILS_REQUIRED` — asks for next missing field conversationally.

**Required fields:** `appGameId`/`appName`, `title`, `dailyBudget`, `durationInDays` (30/60/90), `location`, `fundingSource`, `appLogoUrl`, `screenshotUrls` (public URLs only).

**AI behavior:** Confirm all details with user before calling. Never pass raw card data. Campaign goes under review after creation.

**Example prompt:** “Create a launch campaign for my puzzle game.”

---

## Planned / roadmap tools

The following appear in product documentation and may be added to MCP over time. **Do not invent parameters** — use available tools and official docs until shipped.

| Tool | Purpose |
|------|---------|
| `integrate_mikros` | Engine-specific SDK install and configuration |
| `get_metrics` | DAU, MAU, retention, revenue, ARPU, etc. |
| `get_trends` | Industry benchmarks and genre comparisons |
| `campaign_status` | Campaign review and lifecycle status |

---

See [tool-definitions/tools.json](./tool-definitions/tools.json) for the full registry snapshot.

---

## Common response codes

| Code | Meaning | AI action |
|------|---------|-----------|
| `AUTH_REQUIRED` | Not authenticated | Show login + signup URLs, retry |
| `AUTH_LINK_GENERATION_FAILED` | Bad auth URLs | Retry `mikros_get_login_url` |
| `PROFILE_ONBOARDING_REQUIRED` | Profile incomplete | `complete_profile_onboarding` step by step |
| `OPTIONS_REQUIRED` | Need name/genre/gameplay | Ask in order per LLM_BEHAVIOR |
| `PROJECT_ALREADY_EXISTS` | Duplicate name | Use existing or rename |
| `CAMPAIGN_DETAILS_REQUIRED` | Missing campaign field | Ask next field |
| `DELETE_PROJECT_CONFIRMATION_REQUIRED` | Needs delete confirm | Get exact name confirmation |
| `VALIDATE_INSTALLATION_DETAILS_REQUIRED` | Need project | Ask which project |
| `INSTALLATION_VALIDATION` | Validation results | Interpret checks, guide fixes |

See [schemas/](./schemas/) and [API_REFERENCE.md](./API_REFERENCE.md).
