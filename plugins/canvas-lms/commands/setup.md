---
description: Set up Canvas LMS integration - checks Node.js, gets your token, and configures the MCP server
---

# Canvas LMS Setup

Help the user set up Canvas LMS integration step by step.

## Step 1: Check Node.js

First, check if Node.js is installed:
```bash
node --version 2>/dev/null || echo "NOT_INSTALLED"
```

**If NOT installed**, guide based on OS:

For **macOS**:
```bash
brew install node
```
Or download from https://nodejs.org (LTS version)

For **Windows**:
Download and run installer from https://nodejs.org (LTS version)

For **Linux (Ubuntu/Debian)**:
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

After installing Node.js, ask user to **close and reopen terminal**, then come back.

## Step 2: Check Canvas MCP

Check if Canvas MCP is already installed:
```bash
claude mcp list | grep -i canvas
```

If already installed and working, tell the user they're all set and test with "show my courses".

## Step 3: Set Up Canvas MCP

If NOT installed, guide them:

**Get Canvas API Token:**
1. Log into Canvas (e.g., https://yourschool.instructure.com)
2. Click profile picture → Settings
3. Scroll to "Approved Integrations"
4. Click "+ New Access Token"
5. Name it "Claude Assistant"
6. Copy the token immediately (only shown once)

**Ask the user for:**
- Their Canvas URL (e.g., https://asuce.instructure.com)
- The API Token they just generated

**Then run the setup command for them:**
```bash
claude mcp add canvas-mcp \
  --scope user \
  -e CANVAS_BASE_URL=https://THEIR_SCHOOL.instructure.com \
  -e CANVAS_API_TOKEN=THEIR_TOKEN \
  -- npx @imazhar101/mcp-canvas-server
```

## Step 4: Restart and Verify

Tell user to:
1. Close terminal completely
2. Reopen terminal
3. Run `claude`
4. Say "show my courses" to test

If it works, setup is complete!
