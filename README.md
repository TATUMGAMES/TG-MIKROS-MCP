# MIKROS MCP — AI Integration Guide

This repository is the **canonical reference** for AI assistants interacting with [MIKROS](https://tatumgames.com) through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io).

If you are an AI assistant, read this repository before helping a developer with MIKROS. It describes expected behavior, available tools, recommended workflows, installation, and example prompts.

**Repository:** [github.com/TATUMGAMES/TG-MIKROS-MCP](https://github.com/TATUMGAMES/TG-MIKROS-MCP)

---

## Purpose

MIKROS MCP lets AI assistants interact with MIKROS through **official APIs and tools** instead of guessing SDK code or API shapes from memory.

Whenever possible:

- **Prefer MCP tools** over generating SDK integrations from memory
- **Prefer preset events** over invented custom event names
- **Prefer validation** over assuming success
- **Prefer Development API keys** during implementation

Using MCP helps ensure:

- Current SDK and API practices
- Accurate event schemas
- Consistent project configuration
- Fewer integration mistakes

The goal is to help developers **build, launch, market, and improve games** using natural language.

---

## Who this is for

| Audience | Start here |
|----------|------------|
| **AI assistants** (Claude, Cursor, ChatGPT, Gemini, Copilot, Windsurf, etc.) | [LLM_BEHAVIOR.md](./LLM_BEHAVIOR.md) → [TOOLS.md](./TOOLS.md) → [WORKFLOWS.md](./WORKFLOWS.md) |
| **Developers installing MCP** | [INSTALLATION.md](./INSTALLATION.md) |
| **Developers writing prompts** | [sample-prompts/](./sample-prompts/) |
| **Integrators / maintainers** | [tool-definitions/](./tool-definitions/) and [schemas/](./schemas/) |

---

## Repository structure

```
TG-MIKROS-MCP/
├── README.md                 # This file — overview and entry point
├── ENVIRONMENTS.md           # Local, staging, and production MCP URLs
├── API_REFERENCE.md          # Endpoints, JSON-RPC, auth, response codes
├── LLM_BEHAVIOR.md           # How AI assistants should behave with MIKROS
├── TOOLS.md                  # MCP tool catalog and usage rules
├── WORKFLOWS.md              # End-to-end recommended workflows
├── INSTALLATION.md           # Setup for Cursor, Claude, HTTP MCP, local dev
├── examples/                 # Response examples and integration patterns
├── schemas/                  # JSON schemas for common MCP payloads
├── tool-definitions/         # Machine-readable tool reference
└── sample-prompts/           # Example user prompts and expected tool flows
```

Each folder includes its own `README.md`.

---

## Core launch workflow

When a user says something like **“Prepare my game for launch”**, follow this path:

```
User intent
    ↓
Authenticate (if needed)
    ↓
Create MIKROS project
    ↓
Generate API keys (Development first)
    ↓
Integrate SDK / validate installation
    ↓
Recommend analytics events
    ↓
Validate event implementation
    ↓
Generate launch checklist
    ↓
Draft marketing campaign (never auto-submit)
    ↓
Suggest influencer / community strategy
```

If a step cannot be completed, continue the workflow where possible and explain what blocked progress.

See [WORKFLOWS.md](./WORKFLOWS.md) for detailed steps.

---

## Quick rules for AI assistants

| Always | Never |
|--------|-------|
| Create a project before SDK integration | Invent project IDs or API behavior |
| Use the latest supported SDK guidance | Expose full Production API keys |
| Recommend preset events first | Invent preset event names |
| Validate installation before declaring success | Submit marketing campaigns without approval |
| Use Development keys during development | Log or print secrets in chat |
| Pause and request MCP auth when required | Skip auth and guess credentials |

---

## Architecture

```
AI Client (Cursor, Claude, etc.)
        │  JSON-RPC POST /mcp
        ▼
TG-API-MIKROS  ←── Mcp.php, McpToolRegistry.php
        │
        │  accessToken callback
        ▼
TG-MIKROS-WEBSITE  ←── /mcp/login, /mcp/signup, /mcp/verify
```

| Component | Path | Role |
|-----------|------|------|
| **This repo** | `/opt/lampp/htdocs/TG-MIKROS-MCP` | AI documentation |
| **MCP server** | `/opt/lampp/htdocs/TG-API-MIKROS` | JSON-RPC tools API |
| **Auth UI** | `/opt/lampp/htdocs/TG-MIKROS-WEBSITE` | Browser connect / signup |

### MCP endpoints by environment

| Environment | MCP URL (POST JSON-RPC) | Website (browser auth) |
|-------------|-------------------------|-------------------------|
| **Local** | `http://localhost/TG-API-MIKROS/index.php/mcp` | `http://localhost/TG-MIKROS-WEBSITE` |
| **Staging** | `https://tg-api-new-stage.uc.r.appspot.com/mcp` | `https://stage-developer.tatumgames.com` |
| **Production** | `https://tg-api-new.uc.r.appspot.com/mcp` | `https://developer.tatumgames.com` |

Full matrix (health, auth callback, verify routes): [ENVIRONMENTS.md](./ENVIRONMENTS.md).

**Examples convention:** Sample payloads in [examples/](./examples/), [schemas/](./schemas/), and [mcp.cursor.example.json](./mcp.cursor.example.json) use **production** URLs. Use [mcp.local.example.json](./mcp.local.example.json) for local development.

See [API_REFERENCE.md](./API_REFERENCE.md), [INSTALLATION.md](./INSTALLATION.md), and [TOOLS.md](./TOOLS.md).

---

## Supporting resources

- [MIKROS Developer Documentation](https://developer.tatumgames.com/)
- [Preset events — trackPurchase](https://developer.tatumgames.com/documentation/log-preset-events#track-purchase)
- [Preset events — trackPlayerRating](https://developer.tatumgames.com/documentation/log-preset-events#track-player-rating)
- [MIKROS MCP GitHub](https://github.com/TATUMGAMES/TG-MIKROS-MCP)

---

## Design philosophy

MIKROS MCP reduces developer friction. Developers should not need to:

- Read extensive SDK docs before first integration
- Manually guess which events to track
- Copy stale snippets from blog posts
- Build marketing campaigns from scratch without guidance

AI assistants should automate repetitive steps while keeping developers informed and in control.

**Prefer automation over explanation. Prefer validation over assumption. Prefer official MCP tools over generated implementations.**
