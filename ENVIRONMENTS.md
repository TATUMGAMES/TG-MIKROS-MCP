# MIKROS MCP — Environment URLs

Canonical MCP endpoints for **local**, **staging**, and **production**. Values match `TG-API-MIKROS/index.php` and `TG-MIKROS-WEBSITE/index.php`.

Use the **API MCP URL** in your AI client configuration. Browser **auth** always happens on the **website** host (`WEBURL`).

**Documentation convention:** Sample payloads in [examples/](./examples/) and default config in [mcp.cursor.example.json](./mcp.cursor.example.json) use **production** URLs below.

---

## Quick reference

| Environment | MCP endpoint (configure in AI client) | Website (browser auth) |
|-------------|----------------------------------------|-------------------------|
| **Local** | `http://localhost/TG-API-MIKROS/index.php/mcp` | `http://localhost/TG-MIKROS-WEBSITE` |
| **Staging** | `https://tg-api-new-stage.uc.r.appspot.com/mcp` | `https://stage-developer.tatumgames.com` |
| **Production** | `https://tg-api-new.uc.r.appspot.com/mcp` | `https://developer.tatumgames.com` |

> **Local note:** XAMPP/CodeIgniter requires `/index.php` in API paths. Staging and production App Engine hosts do not.

---

## Full URL matrix

### Local (development)

| Purpose | URL |
|---------|-----|
| API base | `http://localhost/TG-API-MIKROS/index.php` |
| **MCP JSON-RPC** | `http://localhost/TG-API-MIKROS/index.php/mcp` |
| Health check | `http://localhost/TG-API-MIKROS/index.php/mcp/health` |
| Auth callback | `http://localhost/TG-API-MIKROS/index.php/mcp/auth/callback` |
| Website base | `http://localhost/TG-MIKROS-WEBSITE` |
| Connect (login) | `http://localhost/TG-MIKROS-WEBSITE/mcp/login?s={state}` |
| Create account (signup) | `http://localhost/TG-MIKROS-WEBSITE/mcp/signup?s={state}` |
| Email verify | `http://localhost/TG-MIKROS-WEBSITE/mcp/verify/{token}` |
| Auth success | `http://localhost/TG-MIKROS-WEBSITE/mcp/success` |

### Staging

| Purpose | URL |
|---------|-----|
| API base | `https://tg-api-new-stage.uc.r.appspot.com` |
| **MCP JSON-RPC** | `https://tg-api-new-stage.uc.r.appspot.com/mcp` |
| Health check | `https://tg-api-new-stage.uc.r.appspot.com/mcp/health` |
| Auth callback | `https://tg-api-new-stage.uc.r.appspot.com/mcp/auth/callback` |
| Website base | `https://stage-developer.tatumgames.com` |
| Connect (login) | `https://stage-developer.tatumgames.com/mcp/login?s={state}` |
| Create account (signup) | `https://stage-developer.tatumgames.com/mcp/signup?s={state}` |
| Email verify | `https://stage-developer.tatumgames.com/mcp/verify/{token}` |
| Auth success | `https://stage-developer.tatumgames.com/mcp/success` |

### Production

| Purpose | URL |
|---------|-----|
| API base | `https://tg-api-new.uc.r.appspot.com` |
| **MCP JSON-RPC** | `https://tg-api-new.uc.r.appspot.com/mcp` |
| Health check | `https://tg-api-new.uc.r.appspot.com/mcp/health` |
| Auth callback | `https://tg-api-new.uc.r.appspot.com/mcp/auth/callback` |
| Website base | `https://developer.tatumgames.com` |
| Connect (login) | `https://developer.tatumgames.com/mcp/login?s={state}` |
| Create account (signup) | `https://developer.tatumgames.com/mcp/signup?s={state}` |
| Email verify | `https://developer.tatumgames.com/mcp/verify/{token}` |
| Auth success | `https://developer.tatumgames.com/mcp/success` |

---

## Cursor / AI client configuration

Point your MCP client at the **MCP JSON-RPC** URL for your environment:

| Environment | Example config file |
|-------------|---------------------|
| **Production** (default in docs) | [mcp.cursor.example.json](./mcp.cursor.example.json) or [mcp.production.example.json](./mcp.production.example.json) |
| Local | [mcp.local.example.json](./mcp.local.example.json) |
| Staging | [mcp.stage.example.json](./mcp.stage.example.json) |

**Important:** The MCP server URL and the website auth host must belong to the **same environment**. Do not point a production MCP client at staging auth links (or vice versa).

---

## Auth URL behavior

- `mikros_get_login_url` and `AUTH_REQUIRED` responses return `loginUrl` and `signupUrl` built from **`WEBURL`** on the API server for that environment.
- Always use the URLs from the live tool response — do not hard-code `{state}` values.
- The website posts the access token to `{API_BASE}/mcp/auth/callback` for the matching environment.

---

## Health check

```bash
# Local
curl http://localhost/TG-API-MIKROS/index.php/mcp/health

# Staging
curl https://tg-api-new-stage.uc.r.appspot.com/mcp/health

# Production
curl https://tg-api-new.uc.r.appspot.com/mcp/health
```

Expected: JSON with `ok: true`, `service: tg-mikros-mcp`.

---

## Source of truth in code

| Setting | File |
|---------|------|
| API `WEBURL`, `APIURL` per host | `TG-API-MIKROS/index.php` |
| Website `BASE_URL`, `API_URL`, `MIKROS_MCP_CALLBACK` | `TG-MIKROS-WEBSITE/index.php` |
| Auth link generation | `TG-API-MIKROS/application/libraries/McpToolRegistry.php` |

---

## Related docs

- [INSTALLATION.md](./INSTALLATION.md) — setup steps
- [API_REFERENCE.md](./API_REFERENCE.md) — endpoints and JSON-RPC
- [LLM_BEHAVIOR.md](./LLM_BEHAVIOR.md) — auth behavior for AI assistants
