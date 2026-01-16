# Salesforce Plugin for Claude Code

Access your Salesforce org, objects, and records directly through Claude Code using Python.

## Features

- **Python-Based**: Uses simple-salesforce library for reliable access
- **No Third-Party Servers**: Direct API calls, your data stays private
- **Easy Setup**: Guided setup for Python, simple-salesforce, and credentials
- **Privacy-Focused**: Read-only by default, minimal data exposure
- **Full Salesforce Access**: Objects, records, SOQL queries, reports

## Installation

### From Marketplace

```bash
# Add the marketplace
claude plugin marketplace add ASU-LE/salesforce-plugins

# Install the plugin
claude plugin install salesforce@asu-salesforce-plugins
```

### Local Testing

```bash
claude --plugin-dir ./plugins/salesforce
```

## Setup

After installing, run:
```
/salesforce:setup
```

Or just ask about your Salesforce data - Claude will guide you through setup.

## Requirements

1. **Python 3.8+**
2. **simple-salesforce** (`pip3 install simple-salesforce`)
3. **Salesforce credentials**:
   - Username
   - Password
   - Security Token (from Salesforce Settings)
   - Instance URL (e.g., `https://na1.salesforce.com`)

## Usage

Once configured, just talk naturally:

- "Show my Salesforce objects"
- "Query Account records"
- "Find Contacts where LastName is Smith"
- "Show Opportunities closing this month"
- "Describe the Lead object fields"
- "Create an Account" (asks permission)

## Example Operations

```python
# Connect
sf = Salesforce(username, password, security_token)

# Query records
sf.query("SELECT Id, Name FROM Account LIMIT 10")

# Describe object
sf.Account.describe()

# Get record
sf.Account.get('001xxx')

# Create record (with permission)
sf.Account.create({'Name': 'New Company'})
```

## Privacy

- **Read-only by default**: No modifications without explicit permission
- **Direct API**: No data passes through third-party servers
- **Minimal data**: Only fetches what's needed
- **No credential display**: Passwords/tokens are never shown in output

## Troubleshooting

### "simple-salesforce not installed"
```bash
pip3 install simple-salesforce
```

### "Authentication Error"
- Check username, password, and security token
- Ensure your IP is whitelisted or use security token
- Run `/salesforce:setup` to reconfigure

### "Invalid Session"
Session expired. Reconnect by asking any Salesforce question.

## License

MIT

## Sources

- [simple-salesforce GitHub](https://github.com/simple-salesforce/simple-salesforce)
- [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/)
