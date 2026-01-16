# Privacy Guidelines - HubSpot

## Data Handling Rules

1. **Read-Only by Default**: Only perform read operations unless explicitly requested
2. **No Token Display**: Never display or log the access token
3. **Minimal Data**: Only request fields needed for the task
4. **No PII Export**: Don't export contact PII to files without permission
5. **No Bulk Deletes**: Never perform bulk delete operations

## Sensitive Fields

Be careful with these fields:
- `email` - Personal email addresses
- `phone` - Phone numbers
- `mobilephone` - Mobile numbers
- `address` - Physical addresses
- Any custom fields containing PII

## Write Operations

Before any create/update/delete:
1. Confirm the action with the user
2. Show what will be changed
3. Wait for explicit approval

## API Token Security

- Token is stored in environment variable `HUBSPOT_ACCESS_TOKEN`
- Never echo, print, or log the token value
- If token appears in error messages, redact it before displaying
