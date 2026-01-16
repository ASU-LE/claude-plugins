# ASU Learning Enterprise - Claude Code Plugins

A collection of Claude Code plugins for enterprise integrations. One marketplace, all your tools.

## Quick Start

```bash
# Add the marketplace (one time)
claude plugin marketplace add ASU-LE/claude-plugins

# Install the plugins you need
claude plugin install canvas-lms@asu-claude-plugins
claude plugin install airtable@asu-claude-plugins
claude plugin install asana@asu-claude-plugins
claude plugin install salesforce@asu-claude-plugins
claude plugin install hubspot@asu-claude-plugins
```

## Available Plugins

| Plugin | Description | Auth Method |
|--------|-------------|-------------|
| **canvas-lms** | Canvas LMS - courses, modules, pages, assignments, quizzes | MCP + API Token |
| **airtable** | Airtable - bases, tables, records | API Token |
| **asana** | Asana - workspaces, projects, tasks | Personal Access Token |
| **salesforce** | Salesforce CRM - objects, records, SOQL queries | Username/Password/Token |
| **hubspot** | HubSpot CRM - contacts, companies, deals, tickets | Private App Token |

## How It Works

These plugins are **skills** - they auto-activate when you mention relevant topics in conversation:

- *"Show me my Canvas courses"* → Canvas LMS skill activates
- *"List records from Airtable"* → Airtable skill activates
- *"What tasks do I have in Asana?"* → Asana skill activates
- *"Find contacts in Salesforce"* → Salesforce skill activates
- *"Show me HubSpot deals"* → HubSpot skill activates

## Setup Requirements

Each plugin checks prerequisites and guides you through setup:

### Canvas LMS
- Node.js installed
- Canvas API token from your institution

### Airtable
- Python 3.7+
- `pip3 install pyairtable`
- Personal Access Token from [airtable.com/create/tokens](https://airtable.com/create/tokens)

### Asana
- Python 3.7+
- `pip3 install asana`
- Personal Access Token from [app.asana.com/0/my-apps](https://app.asana.com/0/my-apps)

### Salesforce
- Python 3.7+
- `pip3 install simple-salesforce`
- Username, password, and security token

### HubSpot
- Python 3.7+
- `pip3 install hubspot-api-client`
- Private App access token from HubSpot Settings → Integrations → Private Apps

## Environment Variables

Set these in your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
# Canvas (handled by MCP)
# Airtable
export AIRTABLE_API_KEY="patXXXXXX.XXXXXX"

# Asana
export ASANA_ACCESS_TOKEN="1/XXXXXXXXXX"

# Salesforce
export SF_USERNAME="your@email.com"
export SF_PASSWORD="yourpassword"
export SF_SECURITY_TOKEN="XXXXXXXXXX"

# HubSpot
export HUBSPOT_ACCESS_TOKEN="pat-na1-XXXXXXXX"
```

## Privacy & Security

All plugins follow these principles:

- **Read-only by default** - Write operations require explicit confirmation
- **No token display** - API tokens are never shown or logged
- **Minimal data** - Only request fields needed for the task
- **No PII export** - Personal data isn't exported without permission

## Contributing

Want to add a new integration? Follow this structure:

```
plugins/your-service/
├── .claude-plugin/
│   └── plugin.json
├── skills/your-service/
│   ├── SKILL.md          # Main skill with setup checks
│   ├── api-reference.md  # Code patterns
│   └── privacy.md        # Privacy rules
├── commands/
│   └── setup.md          # Optional setup command
└── README.md
```

Then add your plugin to `.claude-plugin/marketplace.json`.

## License

MIT

---

Built by [ASU Learning Enterprise](https://github.com/ASU-LE)
