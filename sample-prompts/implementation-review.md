# Sample prompts: Implementation review

## Prompts

- Review my analytics implementation.
- Is MIKROS working in my build?
- Validate my SDK integration.
- Optimize my event tracking.
- Help improve player retention.
- Help improve monetization.

## Expected tool flow

1. `list_projects`
2. `validate_installation` with `apiKeyType: "dev"` during development
3. `list_custom_events` — see what is actually firing
4. `recommend_events` — fill gaps
5. Actionable recommendations tied to retention or revenue goals

## Example assistant response (opening)

> I'll validate whether your SDK is sending events correctly, then review which events are active and suggest improvements for retention and monetization.
