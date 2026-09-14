# Sample prompts: Marketing campaigns

## Prompts

- Market my game.
- Create an influencer campaign.
- Create a launch campaign.
- Help me promote my game on social media.
- Draft a UA campaign for my mobile game.

## Expected tool flow

1. Authenticate
2. `list_projects`
3. Collect: budget, duration, region, logo URL, screenshot URLs, objectives
4. `create_campaign` — collects missing fields via `CAMPAIGN_DETAILS_REQUIRED`
5. Present summary; campaign is **submitted for review** (not live)
6. Direct to MIKROS Dashboard for billing if needed

## Example assistant response (opening)

> I can draft a marketing campaign for your project. I'll need your daily budget, target region, campaign duration, and public URLs for your logo and screenshots. I'll create a draft for your review — nothing is submitted until you approve.
