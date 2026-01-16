---
description: Set up Airtable integration - install Python, pyairtable, and configure your token
---

# Airtable Setup

Help the user set up Airtable integration step by step.

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

## Step 2: Install pyairtable

```bash
pip3 install pyairtable
```

Verify:
```bash
python3 -c "import pyairtable; print('pyairtable', pyairtable.__version__)"
```

## Step 3: Check Airtable API Key

```bash
echo "AIRTABLE_API_KEY=${AIRTABLE_API_KEY:+SET}"
```

If already configured, test and skip to verification.

## Step 4: Get Airtable Personal Access Token

Guide the user:

1. Go to https://airtable.com/create/tokens
2. Click **"Create new token"**
3. Name it "Claude Assistant"
4. Add scopes:
   - `data.records:read` - to read records
   - `data.records:write` - optional, to create/update
   - `schema.bases:read` - to see base structure
5. Under "Access", add the bases they want to use
6. Click **"Create token"**
7. Copy the token (starts with `pat...`)

## Step 5: Set Environment Variable

**Ask for their token**, then provide the command:

**For macOS/Linux (zsh):**
```bash
echo 'export AIRTABLE_API_KEY="THEIR_TOKEN"' >> ~/.zshrc
source ~/.zshrc
```

**For macOS/Linux (bash):**
```bash
echo 'export AIRTABLE_API_KEY="THEIR_TOKEN"' >> ~/.bashrc
source ~/.bashrc
```

**For Windows PowerShell:**
```powershell
[Environment]::SetEnvironmentVariable("AIRTABLE_API_KEY", "THEIR_TOKEN", "User")
```

## Step 6: Restart and Verify

Tell user to:
1. Close terminal completely
2. Reopen terminal
3. Run `claude`
4. Say "show my Airtable bases" to test

## Step 7: Test

Run this to verify everything works:
```bash
python3 -c "
import os
from pyairtable import Api
api = Api(os.environ['AIRTABLE_API_KEY'])
bases = list(api.bases())
print(f'Success! Found {len(bases)} bases')
for b in bases[:5]:
    print(f'  - {b.name}')
"
```

If it shows bases, setup is complete!
