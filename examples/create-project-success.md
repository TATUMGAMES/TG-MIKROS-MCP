# Example: Successful project creation

## User prompt

> Create a MIKROS project for my Unity game called Heroes vs Villains.

## AI tool sequence

1. `list_project_create_options` — load genre and gameplay lists
2. Ask user for genre and gameplay if not provided
3. `create_project` with `appName`, `gameGenre`, `gameplayType`
4. `generate_api_keys` with `requestType: "dev"`

## Example confirmation (safe fields only)

| Field | Value |
|-------|-------|
| Project | Heroes vs Villains: Nemesis |
| App Game ID | tg-****ddc1 |
| Genre | RPG |
| Gameplay | Competitive |
| Status | Approved |

> I've generated both Development and Production API keys. Let's use the **Development** API key while building and testing your game. Then we'll switch to the Production API key when you're ready to release so your production analytics remain clean.

**Never display:** `appClientId`, `appClientSecret`, full `apiKeyProd`, `apiKeyDev`, or `apiKeyQA`.
