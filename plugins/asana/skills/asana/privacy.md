# Privacy Guidelines

## Core Principles

### 1. Read-Only by Default
- NEVER use create, update, or delete operations without explicit user permission
- If user asks to modify something, confirm first: "This will change [X]. Should I proceed?"
- Write operations require explicit consent

### 2. Minimal Data Fetching
- Always use `opt_fields` parameter to limit returned data
- Only request fields needed for the current task
- Don't fetch "everything just in case"

### 3. No Credential Exposure
- NEVER display or echo `ASANA_ACCESS_TOKEN`
- NEVER include tokens in output or logs
- If a command fails, don't show the full error with token

### 4. Sensitive Data Protection
When displaying tasks:
- Be aware that Asana may contain sensitive project information
- Ask before displaying potentially sensitive task details
- Summarize large task lists rather than dumping all records

### 5. Response Formatting
- Summarize large responses (don't dump raw JSON)
- Format as clean tables when showing tasks
- Truncate long notes with "[...more]" indicator
- Offer to show details rather than overwhelming with data

## What NOT To Do

| Don't | Instead |
|-------|---------|
| Show raw API responses | Format as clean tables/lists |
| Display the access token | Say "using configured credentials" |
| Fetch all tasks without asking | Ask how many/which tasks needed |
| Modify without confirmation | Always confirm writes |
| Show full task dump | Summarize key fields |

## Handling Errors

When API calls fail:
- Don't show full error with token/URL
- Summarize: "Couldn't access [resource]. Check permissions."
- Suggest: "Make sure your token is valid."

## Data Retention

- Don't store Asana data between sessions
- Don't cache task information
- Each request should be fresh from the API
