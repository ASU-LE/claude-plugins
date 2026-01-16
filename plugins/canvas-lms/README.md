# Canvas LMS Plugin for Claude Code

Access your Canvas LMS courses, modules, pages, assignments, and quizzes directly through Claude Code.

## Features

- **Easy Setup**: Guided setup for non-technical users
- **Course Access**: View all your courses
- **Module Navigation**: Browse course modules and content
- **Assignment Tracking**: See assignments and due dates
- **Quiz Information**: View quiz details
- **Privacy-Focused**: Read-only by default, minimal data exposure

## Installation

### Option 1: From Marketplace (Recommended)

```bash
# Add the marketplace
/plugin marketplace add ASU-LE/canvas-lms-plugins

# Install the plugin
/plugin install canvas-lms@canvas-lms-plugins
```

### Option 2: Local Testing

```bash
claude --plugin-dir ./canvas-lms-plugin
```

## Setup

After installing, run:
```
/canvas-lms:setup
```

Or just ask Claude about your Canvas courses - it will guide you through setup if needed.

## Usage

Once configured, just talk naturally:

- "Show my Canvas courses"
- "What's in my Biology course?"
- "Show assignments for CIS 105"
- "What's due this week?"
- "Get the syllabus for my Math class"

## What Gets Installed

This plugin installs the Canvas MCP server globally:
```bash
claude mcp add canvas-mcp \
  --scope user \
  -e CANVAS_BASE_URL=https://yourschool.instructure.com \
  -e CANVAS_API_TOKEN=your-token \
  -- npx @imazhar101/mcp-canvas-server
```

## Privacy

- **Read-only by default**: No modifications without explicit permission
- **Minimal data**: Only fetches what's needed
- **No PII exposure**: Student emails/IDs hidden unless specifically requested
- **No token display**: API tokens are never shown in output

## Troubleshooting

### "Canvas not configured"
Run `/canvas-lms:setup` to configure your Canvas token.

### "Authentication failed"
Your token may have expired. Generate a new one in Canvas Settings.

### MCP not connecting
Try removing and re-adding:
```bash
claude mcp remove canvas-mcp
claude mcp add canvas-mcp --scope user -e CANVAS_BASE_URL=... -e CANVAS_API_TOKEN=... -- npx @imazhar101/mcp-canvas-server
```

## License

MIT
