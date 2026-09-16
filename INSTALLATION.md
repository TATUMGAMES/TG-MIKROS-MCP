# MIKROS MCP Installation Guide

Connect MIKROS MCP to your AI client. The MCP **server** runs on **TG-API-MIKROS**; browser **auth** runs on **TG-MIKROS-WEBSITE**.

| Repo | Local path |
|------|------------|
| Documentation (this repo) | `/opt/lampp/htdocs/TG-MIKROS-MCP` |
| MCP API server | `/opt/lampp/htdocs/TG-API-MIKROS` |
| Auth UI | `/opt/lampp/htdocs/TG-MIKROS-WEBSITE` |

---

## Prerequisites

- XAMPP/Apache running (local) or access to stage/production hosts
- MIKROS account (or create one via MCP signup)
- MCP-compatible AI client (Cursor, Claude Desktop, etc.)

---

## Environment URLs

Configure your AI client with the **MCP JSON-RPC URL** for your environment. Browser auth uses the **website** host in the same environment.

| Environment | MCP endpoint (AI client) | API base | Website (`WEBURL`) |
|-------------|--------------------------|----------|-------------------|
| **Local** | `http://localhost/TG-API-MIKROS/index.php/mcp` | `http://localhost/TG-API-MIKROS/index.php` | `http://localhost/TG-MIKROS-WEBSITE` |
| **Staging** | `https://tg-api-new-stage.uc.r.appspot.com/mcp` | `https://tg-api-new-stage.uc.r.appspot.com` | `https://stage-developer.tatumgames.com` |
| **Production** | `https://tg-api-new.uc.r.appspot.com/mcp` | `https://tg-api-new.uc.r.appspot.com` | `https://developer.tatumgames.com` |

See [ENVIRONMENTS.md](./ENVIRONMENTS.md) for health checks, auth callbacks, and per-route URLs.

---

## Step 1 — Verify MCP API is running

**Local:**

```bash
curl http://localhost/TG-API-MIKROS/index.php/mcp/health
```

**Staging:**

```bash
curl https://tg-api-new-stage.uc.r.appspot.com/mcp/health
```

**Production:**

```bash
curl https://tg-api-new.uc.r.appspot.com/mcp/health
```

Expected: JSON with `ok: true`, `service: tg-mikros-mcp`, `mcpPath`, `authCallback`.

---

## Step 2 — Configure your AI client

Point the client at the **API** MCP endpoint (not the website).

**Cursor** — use the example file for your environment:

| Environment | Example config |
|-------------|----------------|
| **Production** (default in docs) | [mcp.cursor.example.json](./mcp.cursor.example.json) or [mcp.production.example.json](./mcp.production.example.json) |
| Local | [mcp.local.example.json](./mcp.local.example.json) |
| Staging | [mcp.stage.example.json](./mcp.stage.example.json) |

Production example (used in all [examples/](./examples/) samples):

```json
{
  "mcpServers": {
    "mikros": {
      "url": "https://tg-api-new.uc.r.appspot.com/mcp"
    }
  }
}
```

Local: `http://localhost/TG-API-MIKROS/index.php/mcp`  
Staging: `https://tg-api-new-stage.uc.r.appspot.com/mcp`

Merge into Cursor MCP settings and reload MCP. **MCP server and website auth must be the same environment.**

---

## Step 3 — Authenticate via browser

1. Ask your AI client: **“List my MIKROS projects”**
2. If `AUTH_REQUIRED`, open the `loginUrl` and `signupUrl` from the tool response (production example):
   - **Connect:** `https://developer.tatumgames.com/mcp/login?s={state}`
   - **Create account:** `https://developer.tatumgames.com/mcp/signup?s={state}`
   - Local and staging hosts: [ENVIRONMENTS.md](./ENVIRONMENTS.md)
3. Complete sign-in or signup on the **website** MCP pages (not the dashboard)
4. After email verification (new accounts), finish MCP authorization
5. Return to the AI client and retry

The website posts your token to `{API}/mcp/auth/callback`. Sessions are tracked by `mcpSessionId`.

See [API_REFERENCE.md](./API_REFERENCE.md) for headers (`Mcp-Session-Id`, `Authorization`).

---

## Step 4 — Profile onboarding (new accounts)

After first signup, MCP may return `PROFILE_ONBOARDING_REQUIRED`. The AI will collect:

1. Full name
2. Company name
3. Company size

via `complete_profile_onboarding` — one field at a time.

---

## Step 5 — Test a tool call

Example JSON-RPC (optional manual test):

```bash
curl -s -X POST https://tg-api-new.uc.r.appspot.com/mcp \
  -H 'Content-Type: application/json' \
  -H 'Mcp-Session-Id: YOUR_SESSION_ID' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```

For local testing, replace the URL with `http://localhost/TG-API-MIKROS/index.php/mcp` — see [ENVIRONMENTS.md](./ENVIRONMENTS.md).

---

## Website MCP routes (auth only)

| Route | Purpose |
|-------|---------|
| `/mcp/login` | Connect existing account |
| `/mcp/signup` | Create account |
| `/mcp/verify/{token}` | Email verification |
| `/mcp/forgot-password` | Password reset request |
| `/mcp/reset-password/{token}` | Complete password reset |
| `/mcp/success` | Success page |

---

## Developer docs on website

- `/documentation/mikros-mcp/overview`
- `/documentation/mikros-mcp/installation`
- `/documentation/mikros-mcp/faq`

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| `AUTH_REQUIRED` loop | Generate fresh links; complete browser flow within 10 minutes |
| 404 on `/mcp` | Check `TG-API-MIKROS/application/config/routes.php` MCP routes |
| Auth page error | Ensure `TG-MIKROS-WEBSITE` is running and `MIKROS_MCP_CALLBACK` is set |
| `PROFILE_ONBOARDING_REQUIRED` | Complete profile via guided prompts |
| Empty project list | Account needs approved projects |

---

## Security

- Never commit access tokens
- Use **Development** API keys (`generate_api_keys` with `requestType: "dev"`) while building
- MCP auth does **not** create a website dashboard session

---

## Next steps

- [API_REFERENCE.md](./API_REFERENCE.md)
- [WORKFLOWS.md](./WORKFLOWS.md)
- [sample-prompts/](./sample-prompts/)
