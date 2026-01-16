# Privacy Guidelines

## Core Principles

### 1. Read-Only by Default
- Phase 1 of this skill is READ-ONLY
- NEVER use create, update, or delete operations without explicit user permission
- If user asks to modify something, confirm first: "This will change [X]. Should I proceed?"

### 2. Minimal Data Fetching
- Only request fields needed for the current task
- Don't fetch "everything just in case"
- Use specific endpoints, not bulk exports

### 3. No Credential Exposure
- NEVER display or echo `CANVAS_API_TOKEN`
- NEVER include tokens in output or logs
- If a command fails, don't show the full curl command with token

### 4. Student Data Protection
When listing students or submissions:
- Don't include email addresses unless specifically needed
- Don't include student IDs in casual listings
- Summarize grades, don't dump raw grade data
- Ask before showing individual student information

### 5. Response Formatting
- Summarize large responses (don't dump raw JSON)
- Format dates human-readable
- Truncate long content with "[...more]" indicator
- Offer to show details rather than overwhelming with data

## What NOT To Do

| Don't | Instead |
|-------|---------|
| Show raw API responses | Format as clean tables/lists |
| Display the API token | Say "using configured credentials" |
| Fetch all student emails | Only get names unless emails needed |
| Bulk download everything | Fetch specific items on request |
| Modify without confirmation | Always confirm writes |

## Handling Errors

When API calls fail:
- Don't show full error with token/URL
- Summarize: "Couldn't access [resource]. Check if you have permission."
- Suggest: "You might need to ask your Canvas admin for access."

## Data Retention

- Don't store Canvas data between sessions
- Don't cache student information
- Each request should be fresh from the API
