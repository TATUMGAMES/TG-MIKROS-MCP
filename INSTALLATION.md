# MIKROS MCP Installation Guide

Connect MIKROS MCP to your AI client. The MCP **server** runs on **TG-API-MIKROS**; browser **auth** runs on **TG-MIKROS-WEBSITE**.

| Repo | Local path |
|------|------------|
| Documentation (this repo) | `/opt/lampp/htdocs/TG-MIKROS-MCP` |
| MCP API server | `/opt/lampp/htdocs/TG-API-MIKROS` |
| Auth UI | `/opt/lampp/htdocs/TG-MIKROS-WEBSITE` |

---

## Documentation map

| Audience | Where to read |
|----------|----------------|
| **Canonical install reference (this repo)** | This file, [example JSON files](./mcp.cursor.example.json), [ENVIRONMENTS.md](./ENVIRONMENTS.md) |
| **Guided install (copy buttons, client cards)** | [developer.tatumgames.com — Install MIKROS MCP](https://developer.tatumgames.com/documentation/mikros-mcp/installation) — [Quick start (Cursor)](https://developer.tatumgames.com/documentation/mikros-mcp/installation#quick-start), [Other AI desktop clients](https://developer.tatumgames.com/documentation/mikros-mcp/installation#other-ai-desktop-clients) |

When install steps or URLs change, update **this repository first**, then mirror the same URLs and client list on the developer site.

---

## Quick start

There is **no** one-line install command (for example, unlike a local `npx` MCP). MIKROS MCP is **remote**: add a URL to your client, reload MCP, then authenticate in chat.

**Production MCP URL (every client):** `https://tg-api-new.uc.r.appspot.com/mcp`

Configuration **format** depends on the app — see [AI client configuration](#ai-client-configuration) below for each desktop client.

### Cursor desktop (most common copy-paste)

1. **Paste** [mcp.cursor.example.json](./mcp.cursor.example.json) into **Cursor → Settings → MCP** (or your user `mcp.json`).
2. **Reload** MCP in Cursor.
3. **Prompt:** `List my MIKROS projects` — complete browser auth if `AUTH_REQUIRED`.

```json
{
  "mcpServers": {
    "mikros": {
      "url": "https://tg-api-new.uc.r.appspot.com/mcp"
    }
  }
}
```

| Environment | Cursor config file | MCP URL |
|-------------|-------------------|---------|
| **Production** | [mcp.cursor.example.json](./mcp.cursor.example.json) | `https://tg-api-new.uc.r.appspot.com/mcp` |
| **Staging** | [mcp.stage.example.json](./mcp.stage.example.json) | `https://tg-api-new-stage.uc.r.appspot.com/mcp` |
| **Local** | [mcp.local.example.json](./mcp.local.example.json) | `http://localhost/TG-API-MIKROS/index.php/mcp` |

Website mirror: [Install MIKROS MCP](https://developer.tatumgames.com/documentation/mikros-mcp/installation#quick-start) (Cursor) and [Other AI desktop clients](https://developer.tatumgames.com/documentation/mikros-mcp/installation#other-ai-desktop-clients).

---

## AI client configuration

All supported clients connect to the **same MCP endpoint** for a given environment. Only the **settings UI or JSON shape** changes.

| Client | Where to configure | What to enter |
|--------|-------------------|---------------|
| **Cursor desktop** | Settings → MCP or `mcp.json` | [Quick start](#quick-start) — [mcp.cursor.example.json](./mcp.cursor.example.json) |
| **Claude desktop / claude.ai (web)** | Settings → Connectors → Add custom connector (Remote / Web) | **URL only:** `https://tg-api-new.uc.r.appspot.com/mcp` |
| **Claude Code (terminal)** | `.mcp.json` or `claude mcp add` | [mcp.claude-code.example.json](./mcp.claude-code.example.json) — requires `"type": "http"` |
| **Windsurf and other Cursor-like editors** | MCP settings in the editor (often Settings → MCP) | Same JSON as [Quick start](#quick-start) when the editor uses `mcpServers` + `url` |
| **VS Code (MCP extension)** | User `mcp.json` or workspace `.vscode/mcp.json` | [mcp.vscode.example.json](./mcp.vscode.example.json) — uses `"servers"` (not `mcpServers`) |

### Claude desktop or claude.ai (web)

**Where:** Settings → Customize (or Organization) → **Connectors** → **Add custom connector** (Remote / Web).

**Format:** paste the **URL only**:

```
https://tg-api-new.uc.r.appspot.com/mcp
```

1. Start a **new chat**, enable the connector, then ask: `List my MIKROS projects`.

Remote connectors are reached from Anthropic’s cloud; use **production** or **staging** HTTPS URLs, not `localhost`.

### Claude Code (terminal)

**Where:** project `.mcp.json` at the repo root, or `claude mcp add` in a terminal.

**Format:** JSON with `"type": "http"` and `url` (Cursor [Quick start](#quick-start) uses `url` only). Example:

**CLI:**

```bash
claude mcp add --transport http --scope user mikros https://tg-api-new.uc.r.appspot.com/mcp
```

**Or** merge [mcp.claude-code.example.json](./mcp.claude-code.example.json) into `.mcp.json` at your project root:

```json
{
  "mcpServers": {
    "mikros": {
      "type": "http",
      "url": "https://tg-api-new.uc.r.appspot.com/mcp"
    }
  }
}
```

See [mcp.claude-code.example.json](./mcp.claude-code.example.json).

### Windsurf and other Cursor-like editors

**Where:** MCP settings in your editor (same idea as **Cursor → Settings → MCP**).

**Format:** use the [Quick start](#quick-start) JSON when your editor accepts `mcpServers` with a remote `url`. Check your editor’s MCP documentation if tools do not appear after reload.

### Visual Studio Code (MCP extension)

**Where:** run **MCP: Open User Configuration** for profile-wide setup, or create **`.vscode/mcp.json`** in your project for workspace setup.

**Format:** VS Code uses a top-level **`servers`** object (do not copy Cursor/Claude Code `mcpServers` unchanged). Example file: [mcp.vscode.example.json](./mcp.vscode.example.json)

```json
{
  "servers": {
    "mikros": {
      "type": "http",
      "url": "https://tg-api-new.uc.r.appspot.com/mcp"
    }
  }
}
```

See the [VS Code MCP configuration reference](https://code.visualstudio.com/docs/copilot/reference/mcp-configuration).

**MCP server and website auth must be the same environment** (production MCP with production `developer.tatumgames.com` auth, etc.). See [ENVIRONMENTS.md](./ENVIRONMENTS.md).

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

Use the table in [AI client configuration](#ai-client-configuration):

- **Cursor:** [Quick start](#quick-start) and [mcp.cursor.example.json](./mcp.cursor.example.json).
- **Claude desktop / claude.ai (web):** Connectors UI — production URL only.
- **Claude Code (terminal):** [mcp.claude-code.example.json](./mcp.claude-code.example.json) or CLI in [AI client configuration](#claude-code-terminal).
- **Windsurf and similar:** same JSON as Cursor when supported.
- **VS Code (MCP extension):** [mcp.vscode.example.json](./mcp.vscode.example.json) (`servers`, not `mcpServers`).

Reload MCP (or restart the client) after saving.

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
