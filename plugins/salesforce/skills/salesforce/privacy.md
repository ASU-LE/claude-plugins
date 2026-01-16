# Privacy Guidelines

## Core Principles

### 1. Read-Only by Default
- NEVER use create, update, or delete operations without explicit user permission
- If user asks to modify something, confirm first: "This will change [X]. Should I proceed?"
- Write operations require explicit consent

### 2. Minimal Data Fetching
- Only query fields needed for the current task
- Use LIMIT clauses to restrict result sizes
- Don't SELECT * or query "everything just in case"

### 3. No Credential Exposure
- NEVER display or echo `SF_USERNAME`, `SF_PASSWORD`, or `SF_SECURITY_TOKEN`
- NEVER include credentials in output or logs
- If a command fails, don't show connection strings with credentials

### 4. Sensitive Data Protection
When displaying records:
- Be aware that Salesforce contains sensitive business data
- PII (emails, phone numbers, addresses) should be handled carefully
- Ask before displaying potentially sensitive fields
- Summarize large datasets rather than dumping all records

### 5. Response Formatting
- Summarize large responses (don't dump raw JSON)
- Format as clean tables when showing records
- Truncate long text fields with "[...more]" indicator
- Offer to show details rather than overwhelming with data

## What NOT To Do

| Don't | Instead |
|-------|---------|
| Show raw API responses | Format as clean tables/lists |
| Display credentials | Say "using configured credentials" |
| Query all records without asking | Ask how many/which records needed |
| Modify without confirmation | Always confirm writes |
| Show full record dump | Summarize key fields |
| Display connection errors with credentials | Show generic error message |

## Handling Errors

When API calls fail:
- Don't show full error with credentials
- Summarize: "Couldn't connect to Salesforce. Check credentials."
- Suggest: "Run /salesforce:setup to reconfigure."

## Data Retention

- Don't store Salesforce data between sessions
- Don't cache record information
- Each request should be fresh from the API

## SOQL Query Safety

- Always use LIMIT clauses for large objects
- Avoid SELECT * patterns
- Filter with WHERE when possible
- Use query_all() only when explicitly needed
