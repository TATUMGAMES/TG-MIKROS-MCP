# Example: Event recommendations

## User prompt

> Recommend analytics events for my idle RPG.

## AI tool call

```text
recommend_events({
  genre: "RPG",
  gameplayType: "Casual",
  engine: "Unity",
  gameDescription: "Idle RPG with gacha summons and in-app purchases"
})
```

## How to present results

Structure the response as:

### 1. High-priority preset events (every game)

- Session Start / Session End
- Purchase (monetization present)
- First App Launch

### 2. Genre-specific recommendations

- Quest Started / Quest Completed
- Character Created
- Item Crafted

### 3. Why each event matters

One sentence per event tying it to retention, monetization, or progression analysis.

### 4. Next step

Offer to add selected events to the instrumentation plan via `add_recommended_events`, then guide SDK implementation.

**Do not** claim events are already tracking live data until `validate_installation` succeeds.
