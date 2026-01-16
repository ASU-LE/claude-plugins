# Airtable Plugin for Claude Code

Access your Airtable bases, tables, and records directly through Claude Code using Python.

## Features

- **Python-Based**: Uses pyairtable library for clean, reliable access
- **No Third-Party Servers**: Direct API calls, your data stays private
- **Easy Setup**: Guided setup for Python, pyairtable, and token
- **Privacy-Focused**: Read-only by default, minimal data exposure
- **Full Airtable Access**: Bases, tables, records, filtering, sorting
- **Batch Operations**: Create/update multiple records efficiently
- **Formula Support**: Built-in formula builders for complex queries

## Installation

### From Marketplace

```bash
# Add the marketplace
claude plugin marketplace add ASU-LE/airtable-plugins

# Install the plugin
claude plugin install airtable@asu-airtable-plugins
```

### Local Testing

```bash
claude --plugin-dir ./plugins/airtable
```

## Setup

After installing, run:
```
/airtable:setup
```

Or just ask about your Airtable data - Claude will guide you through setup.

## Requirements

1. **Python 3.8+**
2. **pyairtable** (`pip3 install pyairtable`)
3. **Airtable Personal Access Token** from https://airtable.com/create/tokens

## Usage

Once configured, just talk naturally:

- "Show my Airtable bases"
- "What tables are in my Project Tracker?"
- "List records from the Tasks table"
- "Find all tasks where Status is Active"
- "Show me records sorted by Due Date"
- "Create a new record in Tasks" (asks permission)

## Example Operations

```python
# List bases
api.bases()

# Get records
table.all()

# Filter
table.all(formula=match({'Status': 'Active'}))

# Sort
table.all(sort=['-Due Date'])

# Create (with permission)
table.create({'Name': 'New Task', 'Status': 'Active'})
```

## Privacy

- **Read-only by default**: No modifications without explicit permission
- **Direct API**: No data passes through third-party servers
- **Minimal data**: Only fetches what's needed
- **No token display**: API keys are never shown in output

## Troubleshooting

### "pyairtable not installed"
```bash
pip3 install pyairtable
```

### "AIRTABLE_API_KEY not set"
Run `/airtable:setup` to configure your token.

### "401 Unauthorized"
Token expired or lacks permissions. Generate new one at https://airtable.com/create/tokens

### "403 Forbidden"
Token doesn't have access to this base. Edit token and add the base.

## License

MIT

## Sources

- [pyAirtable Documentation](https://pyairtable.readthedocs.io/en/stable/)
- [pyAirtable GitHub](https://github.com/gtalarico/pyairtable)
- [Airtable API](https://airtable.com/developers/web/api/introduction)
