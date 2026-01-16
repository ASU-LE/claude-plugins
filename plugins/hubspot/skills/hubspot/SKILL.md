---
name: hubspot
description: Access HubSpot CRM to view contacts, companies, deals, tickets, and marketing data. Use when user mentions HubSpot, CRM, contacts, companies, deals, leads, or marketing automation. Uses Python hubspot-api-client for reliable access.
---

# HubSpot CRM Client

## First: Check Prerequisites

### Step 1: Check Python
```bash
python3 --version 2>/dev/null || echo "NOT_INSTALLED"
```

**If NOT installed**:
- macOS: `brew install python3`
- Windows: Download from https://python.org

### Step 2: Check hubspot-api-client
```bash
python3 -c "import hubspot; print('OK')" 2>/dev/null || echo "NOT_INSTALLED"
```

**If NOT installed**:
```bash
pip3 install hubspot-api-client
```

### Step 3: Check HubSpot Access Token
```bash
echo "HUBSPOT_ACCESS_TOKEN=${HUBSPOT_ACCESS_TOKEN:+SET}"
```

**If NOT set**, guide the user:

> **To get your HubSpot Access Token:**
> 1. Go to HubSpot → Settings (gear icon)
> 2. Navigate to **Integrations** → **Private Apps**
> 3. Click **Create a private app**
> 4. Name it "Claude Assistant"
> 5. Go to **Scopes** tab and select:
>    - `crm.objects.contacts.read`
>    - `crm.objects.companies.read`
>    - `crm.objects.deals.read`
>    - `crm.objects.owners.read`
>    - (add write scopes only if needed)
> 6. Click **Create app** → **Continue Creating** → **Show token**
> 7. Copy the access token
>
> **Then set it:**
> ```bash
> echo 'export HUBSPOT_ACCESS_TOKEN="pat-na1-xxxxxxxx"' >> ~/.zshrc
> source ~/.zshrc
> ```
>
> **Restart Claude Code after setting the token.**

---

## Quick Examples

**List contacts:**
```bash
python3 -c "
import os
from hubspot import HubSpot

client = HubSpot(access_token=os.environ['HUBSPOT_ACCESS_TOKEN'])
contacts = client.crm.contacts.basic_api.get_page(limit=10)
for c in contacts.results:
    print(f'{c.properties.get(\"firstname\", \"\")} {c.properties.get(\"lastname\", \"\")} - {c.properties.get(\"email\", \"\")}')
"
```

**List companies:**
```bash
python3 -c "
import os
from hubspot import HubSpot

client = HubSpot(access_token=os.environ['HUBSPOT_ACCESS_TOKEN'])
companies = client.crm.companies.basic_api.get_page(limit=10)
for c in companies.results:
    print(f'{c.properties.get(\"name\", \"Unknown\")} - {c.properties.get(\"domain\", \"\")}')
"
```

**List deals:**
```bash
python3 -c "
import os
from hubspot import HubSpot

client = HubSpot(access_token=os.environ['HUBSPOT_ACCESS_TOKEN'])
deals = client.crm.deals.basic_api.get_page(limit=10, properties=['dealname', 'amount', 'dealstage'])
for d in deals.results:
    print(f'{d.properties.get(\"dealname\", \"\")} - \${d.properties.get(\"amount\", \"0\")}')
"
```

See `api-reference.md` for complete patterns.
