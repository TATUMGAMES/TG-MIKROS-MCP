# MIKROS MCP API Reference

Implementation lives in **`TG-API-MIKROS`**. This document matches the code in:

- `application/controllers/Mcp.php`
- `application/libraries/McpToolRegistry.php`
- `application/models/McpAuthModel.php`

Browser auth UI lives in **`TG-MIKROS-WEBSITE`**:

- `application/controllers/McpAuth.php`

---

## Environment URLs

| Environment | MCP endpoint | Website (auth) |
|-------------|--------------|----------------|
| **Local** | `http://localhost/TG-API-MIKROS/index.php/mcp` | `http://localhost/TG-MIKROS-WEBSITE` |
| **Staging** | `https://tg-api-new-stage.uc.r.appspot.com/mcp` | `https://stage-developer.tatumgames.com` |
| **Production** | `https://tg-api-new.uc.r.appspot.com/mcp` | `https://developer.tatumgames.com` |

Full URL matrix (health, callback, verify): [ENVIRONMENTS.md](./ENVIRONMENTS.md).

---

## HTTP endpoints (MIKROS API)

| Method | Path | Purpose |
|--------|------|---------|
| `POST` | `/mcp` | JSON-RPC 2.0 — `initialize`, `tools/list`, `tools/call` |
| `GET` | `/mcp/health` | Service health and paths |
| `POST` | `/mcp/auth/callback` | Save `accessToken` to MCP session (website bridge) |
| `GET`/`POST` | `/mcp/login` | Legacy API-hosted login HTML (prefer website auth) |
| `GET` | `/mcp/debug-auth-links` | Dev-only auth link generator |

Paths are relative to the API base for each environment:

| Environment | API base |
|-------------|----------|
| Local | `http://localhost/TG-API-MIKROS/index.php` |
| Staging | `https://tg-api-new-stage.uc.r.appspot.com` |
| Production | `https://tg-api-new.uc.r.appspot.com` |

---

## JSON-RPC methods

### `initialize`

Returns server info and capabilities.

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "initialize",
  "params": {
    "protocolVersion": "2025-06-18"
  }
}
```

### `notifications/initialized`

Client notification after init. Server returns HTTP 204 (no body).

### `tools/list`

Returns all tools from `McpToolRegistry::listTools()`.

### `tools/call`

```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/call",
  "params": {
    "name": "list_projects",
    "arguments": {}
  }
}
```

---

## Authentication headers

| Header | Purpose |
|--------|---------|
| `Authorization: Bearer <accessToken>` | Optional direct token |
| `Mcp-Session-Id: <mcpSessionId>` | Preferred — resolves token from `mcp_auth_sessions` |

Tool arguments may also include `accessToken` or `mcpSessionId`.

In **development** only, if no token is provided, the registry may use the latest stored development session token.

---

## Browser authorization (MIKROS website)

Auth URLs are built from **`WEBURL`** (website), not the API host:

| URL | Purpose |
|-----|---------|
| `{WEBSITE}/mcp/login?s={state}` | Connect existing account |
| `{WEBSITE}/mcp/signup?s={state}` | Create new account |
| `{WEBSITE}/mcp/verify/{token}` | Email verification after signup |
| `{WEBSITE}/mcp/forgot-password` | MCP password reset |
| `{WEBSITE}/mcp/reset-password/{token}` | Complete password reset |
| `{WEBSITE}/mcp/success` | Post-auth success page |

**Website base by environment:**

| Environment | `WEBURL` |
|-------------|----------|
| Local | `http://localhost/TG-MIKROS-WEBSITE` |
| Staging | `https://stage-developer.tatumgames.com` |
| Production | `https://developer.tatumgames.com` |

The website verifies credentials via the API, then POSTs the `accessToken` to:

```
POST {API_BASE}/mcp/auth/callback
```

Body: `{ "state", "accessToken", "expirationTimeUTC", "email" }`

MCP login state expires in **600 seconds** (10 minutes) by default.

---

## Tool registry (19 tools)

| Tool | Auth |
|------|------|
| `mikros_get_login_url` | No |
| `mikros_signin` | No |
| `mikros_signup` | No |
| `mikros_logout` | Yes |
| `complete_profile_onboarding` | Yes |
| `list_project_create_options` | Yes |
| `create_project` | Yes |
| `list_projects` | Yes |
| `delete_project` | Yes |
| `delete_app` | Yes (alias) |
| `create_campaign` | Yes |
| `generate_api_keys` | Yes |
| `get_app_details` | Yes |
| `validate_installation` | Yes |
| `list_favorite_events` | Yes |
| `list_custom_events` | Yes |
| `recommend_events` | Yes |
| `add_recommended_events` | Yes |
| `create_favorite_event` | Yes |

Full parameters: [TOOLS.md](./TOOLS.md) and [tool-definitions/tools.json](./tool-definitions/tools.json).

---

## Response codes (tool-level)

| Code | Meaning | AI action |
|------|---------|-----------|
| `AUTH_REQUIRED` | Not connected | Show `loginUrl` + `signupUrl`, retry after auth |
| `AUTH_LINK_GENERATION_FAILED` | Bad auth links | Retry `mikros_get_login_url` |
| `PROFILE_ONBOARDING_REQUIRED` | Profile incomplete after signup | Call `complete_profile_onboarding` one field at a time |
| `OPTIONS_REQUIRED` | Need project name / genre / gameplay | Ask in order: name → genre → gameplay |
| `PROJECT_ALREADY_EXISTS` | Duplicate name | Use existing project or pick new name |
| `PROJECT_COMPLETED` | Existing project updated with genre/play type | Continue workflow |
| `CAMPAIGN_DETAILS_REQUIRED` | Missing campaign fields | Ask next field conversationally |
| `DELETE_PROJECT_CONFIRMATION_REQUIRED` | Delete needs confirmation | User must confirm exact name |
| `VALIDATE_INSTALLATION_DETAILS_REQUIRED` | Need project for validation | Ask which project |
| `INSTALLATION_VALIDATION` | Validation result payload | Interpret checks and guide fixes |

---

## Event Knowledge Library

Genre-aware recommendations use:

```
TG-API-MIKROS/application/config/mcp_event_knowledge.json
```

Consumed by `recommend_events` and `add_recommended_events`.

---

## CORS

The MCP controller allows:

- `Access-Control-Allow-Origin: *`
- Methods: `POST`, `GET`, `OPTIONS`
- Headers: `Content-Type`, `Authorization`, `Mcp-Session-Id`

---

## Related docs

- [ENVIRONMENTS.md](./ENVIRONMENTS.md) — local, staging, and production URLs
- [INSTALLATION.md](./INSTALLATION.md) — client setup
- [LLM_BEHAVIOR.md](./LLM_BEHAVIOR.md) — AI rules
- [WORKFLOWS.md](./WORKFLOWS.md) — end-to-end flows
