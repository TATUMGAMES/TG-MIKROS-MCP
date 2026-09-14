# Example: AUTH_REQUIRED

When the user is not connected, tools return an authorization payload.

> **Note:** Sample payloads in this folder use **production** URLs. Other environments: [ENVIRONMENTS.md](../ENVIRONMENTS.md).

`loginUrl` and `signupUrl` use the **website** host for the MCP server environment. Always use the URLs returned by `mikros_get_login_url` or `AUTH_REQUIRED` — do not reuse stale `{state}` values.

## Sample tool response (production)

```json
{
  "code": "AUTH_REQUIRED",
  "message": "Open this authorization URL to grant the MIKROS MCP server access.",
  "loginUrl": "https://developer.tatumgames.com/mcp/login?s=...",
  "signupUrl": "https://developer.tatumgames.com/mcp/signup?s=...",
  "mcpSessionId": "abc123...",
  "instructions": [
    "1. Open the authorization URL in your browser",
    "2. Verify your MIKROS account to grant MCP access",
    "3. Wait for the MCP authorization success page",
    "4. Return here and retry your original request"
  ]
}
```

## How AI should respond

> It looks like you're not connected to MIKROS yet — no problem, I'll help you get set up!
>
> **New here?** Create your free MIKROS account: [Create account](signupUrl)
>
> **Already have an account?** Connect it here: [Connect to MIKROS](loginUrl)
>
> Once you're connected, come back and I'll pick up right where we left off.

## After authorization

Retry the **original tool** (e.g. `list_projects`, `create_project`) without asking the user to re-explain their goal.
