# Salesforce API Reference - Python (simple-salesforce)

Clean Python patterns using the simple-salesforce library.

## Setup

```bash
pip3 install simple-salesforce
```

## Initialization

### Standard Connection
```python
import os
from simple_salesforce import Salesforce

sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)
```

### Sandbox Connection
```python
sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN'],
    domain='test'
)
```

### Custom Domain Connection
```python
sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN'],
    instance_url='https://mycompany.my.salesforce.com'
)
```

### Session ID Connection
```python
sf = Salesforce(
    instance='na1.salesforce.com',
    session_id='SESSION_ID'
)
```

---

## Metadata

### List All Objects
```python
sobjects = sf.describe()['sobjects']
for obj in sobjects:
    if obj['queryable']:
        print(f'{obj["name"]}: {obj["label"]}')
```

### Describe Object (Fields & Metadata)
```python
desc = sf.Account.describe()
print(f'Label: {desc["label"]}')
print(f'Fields: {len(desc["fields"])}')
for field in desc['fields']:
    print(f'  {field["name"]} ({field["type"]})')
```

### Get Object by Name
```python
# For standard objects
desc = sf.Account.describe()
desc = sf.Contact.describe()
desc = sf.Opportunity.describe()

# For custom objects
desc = sf.Custom_Object__c.describe()
```

---

## Querying Records (SOQL)

### Basic Query
```python
result = sf.query("SELECT Id, Name FROM Account LIMIT 10")
for record in result['records']:
    print(record['Name'])
```

### Query with WHERE
```python
result = sf.query("SELECT Id, Name, Email FROM Contact WHERE LastName = 'Smith'")
```

### Query with Multiple Conditions
```python
result = sf.query("""
    SELECT Id, Name, Amount, CloseDate
    FROM Opportunity
    WHERE StageName = 'Prospecting'
    AND Amount > 10000
    ORDER BY Amount DESC
    LIMIT 20
""")
```

### Query All (Handles Pagination)
```python
# Returns all records, handles nextRecordsUrl automatically
result = sf.query_all("SELECT Id, Name FROM Account")
print(f'Total: {result["totalSize"]}')
```

### Query with Related Objects
```python
# Parent relationship (dot notation)
result = sf.query("SELECT Id, Name, Account.Name FROM Contact")

# Child relationship (subquery)
result = sf.query("""
    SELECT Id, Name,
           (SELECT Id, Name FROM Contacts)
    FROM Account
""")
```

### Query Deleted Records
```python
result = sf.query_all("SELECT Id, Name FROM Account WHERE IsDeleted = true", include_deleted=True)
```

---

## SOQL Date Literals

```sql
-- Today
WHERE CreatedDate = TODAY

-- This week/month/quarter/year
WHERE CloseDate = THIS_MONTH
WHERE CloseDate = THIS_QUARTER
WHERE CloseDate = THIS_YEAR

-- Last N days
WHERE CreatedDate = LAST_N_DAYS:30

-- Next N days
WHERE CloseDate = NEXT_N_DAYS:7

-- Date ranges
WHERE CloseDate >= 2024-01-01 AND CloseDate <= 2024-12-31
```

---

## Searching Records (SOSL)

### Basic Search
```python
result = sf.search("FIND {Acme}")
```

### Search with Returning
```python
result = sf.search("""
    FIND {John}
    IN ALL FIELDS
    RETURNING Account(Id, Name), Contact(Id, Name, Email)
""")
```

---

## Record Operations

### Get Record by ID
```python
record = sf.Account.get('001xxx')
print(record['Name'])
```

### Get Record with Specific Fields
```python
record = sf.Account.get('001xxx', fields=['Name', 'Industry', 'Website'])
```

### Get Record by External ID
```python
record = sf.Account.get_by_custom_id('External_Id__c', 'EXT123')
```

---

## Creating Records (Require Permission)

### Create Single Record
```python
result = sf.Account.create({
    'Name': 'New Company',
    'Industry': 'Technology',
    'Website': 'https://example.com'
})
print(f'Created ID: {result["id"]}')
```

### Create with Related Record
```python
# Create Contact with Account relationship
result = sf.Contact.create({
    'FirstName': 'John',
    'LastName': 'Doe',
    'AccountId': '001xxx'
})
```

---

## Updating Records (Require Permission)

### Update Single Record
```python
sf.Account.update('001xxx', {
    'Industry': 'Healthcare',
    'Website': 'https://newsite.com'
})
```

### Upsert (Create or Update)
```python
# Uses External ID field
sf.Account.upsert('External_Id__c/EXT123', {
    'Name': 'Updated Company',
    'Industry': 'Finance'
})
```

---

## Deleting Records (Require Permission)

### Delete Single Record
```python
sf.Account.delete('001xxx')
```

---

## Bulk Operations

### Bulk Query
```python
result = sf.bulk.Account.query("SELECT Id, Name FROM Account")
for record in result:
    print(record['Name'])
```

### Bulk Insert
```python
data = [
    {'Name': 'Company 1', 'Industry': 'Tech'},
    {'Name': 'Company 2', 'Industry': 'Finance'},
    {'Name': 'Company 3', 'Industry': 'Healthcare'}
]
result = sf.bulk.Account.insert(data)
```

### Bulk Update
```python
data = [
    {'Id': '001xxx', 'Industry': 'Updated'},
    {'Id': '001yyy', 'Industry': 'Updated'}
]
result = sf.bulk.Account.update(data)
```

### Bulk Upsert
```python
data = [
    {'External_Id__c': 'EXT1', 'Name': 'Company 1'},
    {'External_Id__c': 'EXT2', 'Name': 'Company 2'}
]
result = sf.bulk.Account.upsert(data, 'External_Id__c')
```

### Bulk Delete
```python
data = [{'Id': '001xxx'}, {'Id': '001yyy'}]
result = sf.bulk.Account.delete(data)
```

---

## Common Queries by Object

### Accounts
```python
sf.query("""
    SELECT Id, Name, Industry, Type, Website, Phone,
           BillingCity, BillingState
    FROM Account
    WHERE Type = 'Customer'
    ORDER BY Name
    LIMIT 50
""")
```

### Contacts
```python
sf.query("""
    SELECT Id, FirstName, LastName, Email, Phone,
           Account.Name, Title
    FROM Contact
    WHERE Email != null
    ORDER BY LastName
    LIMIT 50
""")
```

### Leads
```python
sf.query("""
    SELECT Id, Name, Company, Status, LeadSource, Email
    FROM Lead
    WHERE IsConverted = false AND Status != 'Closed'
    ORDER BY CreatedDate DESC
    LIMIT 50
""")
```

### Opportunities
```python
sf.query("""
    SELECT Id, Name, StageName, Amount, CloseDate,
           Account.Name, Owner.Name
    FROM Opportunity
    WHERE IsClosed = false
    ORDER BY CloseDate
    LIMIT 50
""")
```

### Cases
```python
sf.query("""
    SELECT Id, CaseNumber, Subject, Status, Priority,
           Account.Name, Contact.Name
    FROM Case
    WHERE IsClosed = false
    ORDER BY Priority, CreatedDate
    LIMIT 50
""")
```

### Tasks
```python
sf.query("""
    SELECT Id, Subject, Status, Priority, ActivityDate,
           Who.Name, What.Name
    FROM Task
    WHERE IsClosed = false
    ORDER BY ActivityDate
    LIMIT 50
""")
```

---

## Error Handling

```python
from simple_salesforce.exceptions import SalesforceAuthenticationFailed, SalesforceMalformedRequest

try:
    result = sf.query("SELECT Invalid FROM Account")
except SalesforceAuthenticationFailed:
    print("Authentication failed - check credentials")
except SalesforceMalformedRequest as e:
    print(f"Query error: {e}")
except Exception as e:
    print(f"Error: {e}")
```

---

## API Limits

### Check Limits
```python
limits = sf.limits()
print(f'Daily API Requests: {limits["DailyApiRequests"]["Remaining"]}/{limits["DailyApiRequests"]["Max"]}')
```

---

## Full Example

```python
import os
from simple_salesforce import Salesforce

# Connect
sf = Salesforce(
    username=os.environ['SF_USERNAME'],
    password=os.environ['SF_PASSWORD'],
    security_token=os.environ['SF_SECURITY_TOKEN']
)

print(f"Connected to: {sf.sf_instance}")

# Query open opportunities closing this month
opps = sf.query("""
    SELECT Id, Name, Amount, CloseDate, StageName, Account.Name
    FROM Opportunity
    WHERE CloseDate = THIS_MONTH AND IsClosed = false
    ORDER BY Amount DESC
    LIMIT 10
""")

print(f"\nOpportunities closing this month: {opps['totalSize']}")
print("-" * 60)

total = 0
for opp in opps['records']:
    amount = opp.get('Amount') or 0
    total += amount
    account = opp.get('Account', {}).get('Name', 'No Account')
    print(f"${amount:,.0f} - {opp['Name']}")
    print(f"  Account: {account} | Stage: {opp['StageName']}")

print("-" * 60)
print(f"Total Pipeline: ${total:,.0f}")
```
