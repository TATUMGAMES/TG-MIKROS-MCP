# Sample prompts

Example user requests and the MIKROS MCP tools AI assistants should use.

MCP URLs: [ENVIRONMENTS.md](../ENVIRONMENTS.md). Illustrative examples in this repo use **production** (`https://tg-api-new.uc.r.appspot.com/mcp`).

| Prompt file | User says | Primary tools |
|-------------|-----------|---------------|
| [analytics-integration.md](./analytics-integration.md) | Add analytics to my game | `create_project`, `generate_api_keys`, `recommend_events`, `validate_installation` |
| [launch-preparation.md](./launch-preparation.md) | Prepare my game for launch | Full launch workflow |
| [event-recommendations.md](./event-recommendations.md) | Recommend analytics events | `recommend_events`, `add_recommended_events` |
| [kpis-and-metrics.md](./kpis-and-metrics.md) | Show my KPIs | `list_projects`, `list_favorite_events`, `list_custom_events` |
| [marketing-campaigns.md](./marketing-campaigns.md) | Market my game | `create_campaign` |
| [implementation-review.md](./implementation-review.md) | Review my analytics setup | `validate_installation`, `list_custom_events` |

AI assistants should recognize **similar phrasing** and map to the same tool flows.
