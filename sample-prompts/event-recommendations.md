# Sample prompts: Event recommendations

## Prompts

- Recommend analytics events for my idle RPG.
- What events should I track for a battle royale?
- Track player progression in my platformer.
- Which MIKROS events matter for a puzzle game with ads?

## Expected tool flow

1. `list_projects` or use provided context
2. `recommend_events` with genre, gameplay, engine, description
3. Present preset events first (Session Start/End, Purchase if monetized)
4. `add_recommended_events` if user selects specific items
5. Link to [trackPurchase](https://developer.tatumgames.com/documentation/log-preset-events#track-purchase) and [trackPlayerRating](https://developer.tatumgames.com/documentation/log-preset-events#track-player-rating) when relevant

## Example assistant response (opening)

> I'll recommend events based on your game's genre and monetization model, starting with high-value preset events like Session Start, Session End, and Purchase if you sell IAP.
