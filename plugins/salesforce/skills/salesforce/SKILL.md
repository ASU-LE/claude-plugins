---
name: salesforce
description: Access Salesforce CRM to view accounts, contacts, leads, opportunities, and custom objects. Use when user mentions Salesforce, CRM, accounts, contacts, leads, opportunities, or sales data. Uses Python simple-salesforce library for reliable access.
---

# Salesforce Client

You are a Salesforce client that helps users access their Salesforce org using Python with the simple-salesforce library.

## First: Check Prerequisites

Before ANY Salesforce operation, run these checks in order:

### Step 1: Check Python

```bash
python3 --version 2>/dev/null || echo "NOT_INSTALLED"
```

**If NOT installed**, guide based on OS:

For **macOS**:
```bash
brew install python3
```

For **Windows**:
Download from https://python.org (add to PATH during install)

For **Linux**:
```bash
sudo apt-get install python3 python3-pip
```

### Step 2: Check simple-salesforce

```bash
python3 -c "from simple_salesforce import Salesforce; print('OK')" 2>/dev/null || echo "NOT_INSTALLED"
```

**If NOT installed**:
```bash
pip3 install simple-salesforce
```

### Step 3: Check Salesforce Credentials

```bash
echo "SF_USERNAME=${SF_USERNAME:+SET}"
echo "SF_PASSWORD=${SF_PASSWORD:+SET}"
echo "SF_SECURITY_TOKEN=${SF_SECURITY_TOKEN:+SET}"
```

**If NOT configured**, guide the user:

> **Salesforce is not configured yet. Let me help you set it up.**
>
> **Step 1: Get your Security Token**
> 1. Log into Salesforce
> 2. Click profile icon → **Settings**
> 3. Search **"Reset My Security Token"**
> 4. Click Reset and check your email
>
> **Step 2: Set environment variables**
> ```bash
> echo 'export SF_USERNAME="your_email@company.com"' >> ~/.zshrc
> echo 'export SF_PASSWORD="your_password"' >> ~/.zshrc
> echo 'export SF_SECURITY_TOKEN="token_from_email"' >> ~/.zshrc
> source ~/.zshrc
> ```
>
> **Step 3: Restart Claude Code** and come back

Then STOP and wait for user to complete setup.

## Python Code Patterns

Use these Python patterns for Salesforce operations. Always use `python3 -c` for quick operations.

### Initialize Connection

```python
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
```

### Test Connection

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
"
```

### List All Objects

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
sobjects = sf.describe()['sobjects']
for obj in sobjects[:20]:
    if obj['queryable']:
        print(f'{obj[\"name\"]} - {obj[\"label\"]}')
"
```

### Describe Object (Show Fields)

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
desc = sf.OBJECT_NAME.describe()
print(f'{desc[\"label\"]} ({desc[\"name\"]})')
print('Fields:')
for field in desc['fields'][:20]:
    print(f'  - {field[\"name\"]} ({field[\"type\"]}): {field[\"label\"]}')
"
```

### Query Records (SOQL)

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
result = sf.query('SELECT Id, Name FROM Account LIMIT 10')
print(f'Found {result[\"totalSize\"]} records')
for record in result['records']:
    print(f'  {record[\"Id\"]}: {record[\"Name\"]}')
"
```

### Query with WHERE Clause

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
result = sf.query(\"SELECT Id, Name, Email FROM Contact WHERE LastName = 'Smith'\")
for record in result['records']:
    print(f'{record[\"Name\"]} - {record.get(\"Email\", \"No email\")}')
"
```

### Query All Records (Handle Pagination)

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
result = sf.query_all('SELECT Id, Name FROM Account')
print(f'Total records: {result[\"totalSize\"]}')
"
```

### Get Single Record

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
record = sf.Account.get('RECORD_ID')
for key, value in record.items():
    if value and key != 'attributes':
        print(f'{key}: {value}')
"
```

### Search (SOSL)

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
result = sf.search('FIND {SEARCH_TERM} IN ALL FIELDS RETURNING Account(Id, Name), Contact(Id, Name)')
for obj_result in result['searchRecords']:
    print(f'{obj_result[\"attributes\"][\"type\"]}: {obj_result.get(\"Name\", obj_result[\"Id\"])}')
"
```

## Common Queries

### Accounts
```sql
SELECT Id, Name, Industry, Website, Phone FROM Account LIMIT 20
```

### Contacts
```sql
SELECT Id, FirstName, LastName, Email, Account.Name FROM Contact LIMIT 20
```

### Leads
```sql
SELECT Id, Name, Company, Status, Email FROM Lead WHERE IsConverted = false LIMIT 20
```

### Opportunities
```sql
SELECT Id, Name, StageName, Amount, CloseDate, Account.Name FROM Opportunity WHERE IsClosed = false
```

### Opportunities Closing This Month
```sql
SELECT Id, Name, Amount, CloseDate FROM Opportunity WHERE CloseDate = THIS_MONTH AND IsClosed = false
```

## Write Operations (Require Explicit Permission)

### Create Record

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
result = sf.Account.create({'Name': 'New Company', 'Industry': 'Technology'})
print(f'Created: {result[\"id\"]}')
"
```

### Update Record

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
sf.Account.update('RECORD_ID', {'Industry': 'Healthcare'})
print('Updated')
"
```

### Delete Record

```bash
python3 -c "
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
sf.Account.delete('RECORD_ID')
print('Deleted')
"
```

## Privacy Rules (ALWAYS FOLLOW)

See [privacy.md](privacy.md) for complete rules. Key points:

1. **Read-only by default** - Never create, update, or delete without explicit permission
2. **Minimal data** - Only query fields needed for the task
3. **No credential display** - NEVER echo passwords, tokens, or usernames
4. **Summarize, don't dump** - Format responses cleanly

## Common Operations

| User says... | Action |
|--------------|--------|
| "Show my Salesforce objects" | List queryable objects |
| "Describe Account" | Show Account fields |
| "Query Accounts" | SOQL query |
| "Find contacts named Smith" | Query with WHERE |
| "Create an Account" | Create (ask permission first) |
| "Update this record" | Update (ask permission first) |

## Displaying Results

Format as clean tables:

**Good:**
```
Accounts:
┌──────────────────────┬──────────────┬────────────────────┐
│ Name                 │ Industry     │ Phone              │
├──────────────────────┼──────────────┼────────────────────┤
│ Acme Corporation     │ Technology   │ (555) 123-4567     │
│ Global Industries    │ Manufacturing│ (555) 987-6543     │
└──────────────────────┴──────────────┴────────────────────┘
```

**Bad:**
```json
{"totalSize":2,"records":[{"attributes":{"type":"Account"...
```

## Reference

- [API Reference](api-reference.md) - All Python patterns
- [Privacy Rules](privacy.md) - Data handling guidelines

Sources: [simple-salesforce GitHub](https://github.com/simple-salesforce/simple-salesforce), [Salesforce REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/)
