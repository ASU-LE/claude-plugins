---
description: Set up Asana integration - install Python, asana library, and configure your token
---

# Asana Setup

Help the user set up Asana integration step by step.

## Step 1: Check Python

```bash
python3 --version 2>/dev/null || echo "NOT_INSTALLED"
```

**If NOT installed**, guide based on OS:

For **macOS**:
```bash
brew install python3
```

For **Windows**:
Download from https://python.org (check "Add to PATH" during install)

For **Linux**:
```bash
sudo apt-get install python3 python3-pip
```

After installing, ask user to close and reopen terminal.

## Step 2: Install asana library

```bash
pip3 install asana
```

Verify:
```bash
python3 -c "import asana; print('asana library installed')"
```

## Step 3: Check Asana Access Token

```bash
echo "ASANA_ACCESS_TOKEN=${ASANA_ACCESS_TOKEN:+SET}"
```

If already configured, test and skip to verification.

## Step 4: Get Asana Personal Access Token

Guide the user:

1. Go to https://app.asana.com/0/my-apps
2. Click **"Create new token"**
3. Name it "Claude Assistant"
4. Copy the token that appears

**Important**: Asana tokens don't expire but can be revoked anytime.

## Step 5: Set Environment Variable

**Ask for their token**, then provide the command:

**For macOS/Linux (zsh):**
```bash
echo 'export ASANA_ACCESS_TOKEN="THEIR_TOKEN"' >> ~/.zshrc
source ~/.zshrc
```

**For macOS/Linux (bash):**
```bash
echo 'export ASANA_ACCESS_TOKEN="THEIR_TOKEN"' >> ~/.bashrc
source ~/.bashrc
```

**For Windows PowerShell:**
```powershell
[Environment]::SetEnvironmentVariable("ASANA_ACCESS_TOKEN", "THEIR_TOKEN", "User")
```

## Step 6: Restart and Verify

Tell user to:
1. Close terminal completely
2. Reopen terminal
3. Run `claude`
4. Say "show my Asana workspaces" to test

## Step 7: Test

Run this to verify everything works:
```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
me = client.users.me()
print(f'Connected as: {me[\"name\"]}')
workspaces = list(client.workspaces.find_all())
print(f'Found {len(workspaces)} workspaces')
for w in workspaces[:5]:
    print(f'  - {w[\"name\"]}')
"
```

If it shows workspaces, setup is complete!
