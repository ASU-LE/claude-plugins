# Asana Plugin for Claude Code

Access your Asana workspaces, projects, and tasks directly through Claude Code using Python.

## Features

- **Python-Based**: Uses official asana library for reliable access
- **No Third-Party Servers**: Direct API calls, your data stays private
- **Easy Setup**: Guided setup for Python, asana library, and token
- **Privacy-Focused**: Read-only by default, minimal data exposure
- **Full Asana Access**: Workspaces, projects, tasks, users, sections

## Installation

### From Marketplace

```bash
# Add the marketplace
claude plugin marketplace add ASU-LE/asana-plugins

# Install the plugin
claude plugin install asana@asu-asana-plugins
```

### Local Testing

```bash
claude --plugin-dir ./plugins/asana
```

## Setup

After installing, run:
```
/asana:setup
```

Or just ask about your Asana data - Claude will guide you through setup.

## Requirements

1. **Python 3.8+**
2. **asana** (`pip3 install asana`)
3. **Asana Personal Access Token** from https://app.asana.com/0/my-apps

## Usage

Once configured, just talk naturally:

- "Show my Asana workspaces"
- "What projects are in my workspace?"
- "List tasks in the Marketing project"
- "Show incomplete tasks assigned to me"
- "Find tasks due this week"
- "Create a new task in [project]" (asks permission)

## Example Operations

```python
# List workspaces
client.workspaces.find_all()

# Get projects
client.projects.find_all({'workspace': workspace_gid})

# Get tasks
client.tasks.find_all({'project': project_gid})

# Filter incomplete tasks
client.tasks.find_all({'project': gid, 'completed_since': 'now'})
```

## Privacy

- **Read-only by default**: No modifications without explicit permission
- **Direct API**: No data passes through third-party servers
- **Minimal data**: Only fetches what's needed
- **No token display**: API keys are never shown in output

## Troubleshooting

### "asana not installed"
```bash
pip3 install asana
```

### "ASANA_ACCESS_TOKEN not set"
Run `/asana:setup` to configure your token.

### "401 Unauthorized"
Token expired or invalid. Generate new one at https://app.asana.com/0/my-apps

## License

MIT

## Sources

- [Asana Python Client](https://github.com/Asana/python-asana)
- [Asana API Documentation](https://developers.asana.com/docs)
