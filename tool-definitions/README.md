# Tool definitions

Machine-readable reference for MIKROS MCP tools.

| File | Description |
|------|-------------|
| [tools.json](./tools.json) | Full tool catalog (names, auth, descriptions) |

## Source of truth

| Source | Path |
|--------|------|
| Tool registry | `TG-API-MIKROS/application/libraries/McpToolRegistry.php` |
| MCP controller | `TG-API-MIKROS/application/controllers/Mcp.php` |
| Auth sessions | `TG-API-MIKROS/application/models/McpAuthModel.php` |
| Event library | `TG-API-MIKROS/application/config/mcp_event_knowledge.json` |
| Website auth | `TG-MIKROS-WEBSITE/application/controllers/McpAuth.php` |

Update `tools.json` when `McpToolRegistry.php` changes.

## Environment URLs

| Environment | MCP URL |
|-------------|---------|
| Local | `http://localhost/TG-API-MIKROS/index.php/mcp` |
| Staging | `https://tg-api-new-stage.uc.r.appspot.com/mcp` |
| Production | `https://tg-api-new.uc.r.appspot.com/mcp` |

Also in `tools.json` → `transport.environments`. Full matrix: [ENVIRONMENTS.md](../ENVIRONMENTS.md).

**Convention:** Code samples below use the **production** MCP URL.

## JSON-RPC (HTTP MCP API)

```http
POST https://tg-api-new.uc.r.appspot.com/mcp
Content-Type: application/json

{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}
```

```http
POST https://tg-api-new.uc.r.appspot.com/mcp
Content-Type: application/json

{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"list_projects","arguments":{}}}
```

Pass `Mcp-Session-Id` header or `mcpSessionId` in arguments when authenticated.
