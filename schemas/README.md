# Schemas

JSON schema references for common MIKROS MCP payloads.

MCP endpoints by environment: [ENVIRONMENTS.md](../ENVIRONMENTS.md).

**Convention:** `examples` blocks in schemas use **production** URLs (`https://tg-api-new.uc.r.appspot.com/mcp`, `https://developer.tatumgames.com`).

| File | Description |
|------|-------------|
| [auth-required.json](./auth-required.json) | MCP authentication needed |
| [profile-onboarding-required.json](./profile-onboarding-required.json) | Profile fields missing after signup |
| [options-required.json](./options-required.json) | Project name / genre / gameplay needed |
| [campaign-details-required.json](./campaign-details-required.json) | Campaign fields missing |
| [tool-result.json](./tool-result.json) | Generic API result wrapper |

These schemas describe **logical shapes** for documentation and AI parsing. Runtime responses may include additional fields from the MIKROS API.
