# Sample prompts: KPIs and metrics

## Prompts

- Show my top KPIs.
- Show my most interesting KPIs.
- How is player retention looking?
- What's my revenue this month?
- Compare my retention to industry benchmarks.

## Expected tool flow

1. Authenticate
2. `list_projects` — select project
3. `list_favorite_events` and/or `list_custom_events` with date range
4. Summarize insights and suggest improvements
5. For benchmarks — use available data; `get_trends` when shipped

## Example assistant response (opening)

> I'll pull your project's analytics charts and summarize the KPIs that matter most for your game type. If you have a specific date range in mind, let me know.
