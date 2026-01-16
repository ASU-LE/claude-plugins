---
description: Set up Salesforce integration - install Python, simple-salesforce, and configure credentials
---

# Salesforce Setup

Help the user set up Salesforce integration step by step.

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

## Step 2: Install simple-salesforce

```bash
pip3 install simple-salesforce
```

Verify:
```bash
python3 -c "from simple_salesforce import Salesforce; print('simple-salesforce installed')"
```

## Step 3: Check Salesforce Credentials

```bash
echo "SF_USERNAME=${SF_USERNAME:+SET}"
echo "SF_PASSWORD=${SF_PASSWORD:+SET}"
echo "SF_SECURITY_TOKEN=${SF_SECURITY_TOKEN:+SET}"
```

If all are configured, test and skip to verification.

## Step 4: Get Salesforce Security Token

Guide the user:

1. Log into Salesforce
2. Click your profile icon → **Settings**
3. In Quick Find, search **"Reset My Security Token"**
4. Click **Reset Security Token**
5. Check email for the new token

**Note**: If you don't see this option, your admin may have disabled it. Contact your Salesforce admin.

## Step 5: Gather Credentials

Ask the user for:
1. **Username** - Their Salesforce login email
2. **Password** - Their Salesforce password
3. **Security Token** - From email in Step 4
4. **Instance URL** (optional) - e.g., `https://na1.salesforce.com` (defaults to login.salesforce.com)

## Step 6: Set Environment Variables

**For macOS/Linux (zsh):**
```bash
echo 'export SF_USERNAME="user@company.com"' >> ~/.zshrc
echo 'export SF_PASSWORD="your_password"' >> ~/.zshrc
echo 'export SF_SECURITY_TOKEN="your_token"' >> ~/.zshrc
source ~/.zshrc
```

**For macOS/Linux (bash):**
```bash
echo 'export SF_USERNAME="user@company.com"' >> ~/.bashrc
echo 'export SF_PASSWORD="your_password"' >> ~/.bashrc
echo 'export SF_SECURITY_TOKEN="your_token"' >> ~/.bashrc
source ~/.bashrc
```

**For Windows PowerShell:**
```powershell
[Environment]::SetEnvironmentVariable("SF_USERNAME", "user@company.com", "User")
[Environment]::SetEnvironmentVariable("SF_PASSWORD", "your_password", "User")
[Environment]::SetEnvironmentVariable("SF_SECURITY_TOKEN", "your_token", "User")
```

**Optional**: If using a sandbox or custom domain:
```bash
export SF_DOMAIN="test"  # For sandbox
# or
export SF_INSTANCE_URL="https://mycompany.my.salesforce.com"
```

## Step 7: Restart and Verify

Tell user to:
1. Close terminal completely
2. Reopen terminal
3. Run `claude`
4. Say "show my Salesforce objects" to test

## Step 8: Test

Run this to verify everything works:
```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
print(f'Connected to: {sf.sf_instance}')
# Quick test - describe Account
acc = sf.Account.describe()
print(f'Account object has {len(acc[\"fields\"])} fields')
print('Connection successful!')
"
```

If it shows connection info, setup is complete!

## Sandbox/Test Environment

For sandbox environments, use:
```bash
export SF_DOMAIN="test"
```

Or specify instance URL directly:
```bash
export SF_INSTANCE_URL="https://test.salesforce.com"
```
