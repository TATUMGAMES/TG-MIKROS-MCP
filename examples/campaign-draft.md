# Example: Marketing campaign draft

## User prompt

> Create a launch campaign for my puzzle game with a $50 daily budget.

## AI tool sequence

1. `list_projects` — identify project
2. Collect missing fields: duration, region, logo URL, screenshot URLs, objectives
3. `create_campaign` — returns **draft**
4. Present summary for review

## Example draft summary

| Field | Value |
|-------|-------|
| Project | Block Cascade |
| Title | Launch Week — Block Cascade |
| Daily budget | $50 |
| Duration | 30 days |
| Region | United States |
| Status | **Draft** |

## Required AI closing

> Your campaign has been **created and submitted for review**. It is not live yet.
>
> Please confirm budget, duration, target region, and creative assets were correct. If billing is not set up, complete payment in the **MIKROS Dashboard**.

**Never** call `create_campaign` without explicit user intent and confirmed details. MCP does not upload files — only public URLs.
