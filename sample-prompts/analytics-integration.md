# Sample prompts: Analytics integration

## Prompts

- Add analytics to my Unity game.
- Add analytics to my Unreal project.
- Add analytics to my Godot game.
- Integrate MIKROS into my mobile game.
- Set up MIKROS tracking for my Steam game.

## Expected tool flow

1. `mikros_get_login_url` — if not authenticated
2. `list_projects` — check for existing project
3. `create_project` + `list_project_create_options` — if no project exists
4. `generate_api_keys` — `requestType: "dev"`
5. Guide SDK installation from [official docs](https://developer.tatumgames.com/)
6. `recommend_events`
7. `validate_installation`

## Example assistant response (opening)

> I'll help you add MIKROS Analytics to your Unity project. First I'll check whether you already have a MIKROS project, then we'll generate a Development API key and recommend the right events for your game type.
