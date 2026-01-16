# HubSpot API Reference - Python (hubspot-api-client)

Official HubSpot Python client library patterns.

## Setup

```bash
pip3 install hubspot-api-client
```

## Initialization

```python
import os
from hubspot import HubSpot

client = HubSpot(access_token=os.environ['HUBSPOT_ACCESS_TOKEN'])
```

---

## Contacts

### List Contacts
```python
contacts = client.crm.contacts.basic_api.get_page(
    limit=100,
    properties=['firstname', 'lastname', 'email', 'phone', 'company']
)
for c in contacts.results:
    print(f'{c.id}: {c.properties}')
```

### Get Contact by ID
```python
contact = client.crm.contacts.basic_api.get_by_id(
    contact_id='123',
    properties=['firstname', 'lastname', 'email', 'phone']
)
```

### Search Contacts
```python
from hubspot.crm.contacts import PublicObjectSearchRequest

search = PublicObjectSearchRequest(
    filter_groups=[{
        'filters': [{
            'propertyName': 'email',
            'operator': 'CONTAINS_TOKEN',
            'value': '@example.com'
        }]
    }],
    properties=['firstname', 'lastname', 'email'],
    limit=50
)
results = client.crm.contacts.search_api.do_search(search)
```

### Search by Name
```python
search = PublicObjectSearchRequest(
    filter_groups=[{
        'filters': [{
            'propertyName': 'firstname',
            'operator': 'EQ',
            'value': 'John'
        }]
    }],
    properties=['firstname', 'lastname', 'email']
)
results = client.crm.contacts.search_api.do_search(search)
```

---

## Companies

### List Companies
```python
companies = client.crm.companies.basic_api.get_page(
    limit=100,
    properties=['name', 'domain', 'industry', 'city', 'state']
)
```

### Get Company by ID
```python
company = client.crm.companies.basic_api.get_by_id(
    company_id='456',
    properties=['name', 'domain', 'industry', 'numberofemployees']
)
```

### Search Companies
```python
from hubspot.crm.companies import PublicObjectSearchRequest

search = PublicObjectSearchRequest(
    filter_groups=[{
        'filters': [{
            'propertyName': 'industry',
            'operator': 'EQ',
            'value': 'Technology'
        }]
    }],
    properties=['name', 'domain', 'industry']
)
results = client.crm.companies.search_api.do_search(search)
```

---

## Deals

### List Deals
```python
deals = client.crm.deals.basic_api.get_page(
    limit=100,
    properties=['dealname', 'amount', 'dealstage', 'closedate', 'pipeline']
)
```

### Get Deal by ID
```python
deal = client.crm.deals.basic_api.get_by_id(
    deal_id='789',
    properties=['dealname', 'amount', 'dealstage', 'closedate']
)
```

### Search Deals by Stage
```python
from hubspot.crm.deals import PublicObjectSearchRequest

search = PublicObjectSearchRequest(
    filter_groups=[{
        'filters': [{
            'propertyName': 'dealstage',
            'operator': 'EQ',
            'value': 'closedwon'
        }]
    }],
    properties=['dealname', 'amount', 'closedate']
)
results = client.crm.deals.search_api.do_search(search)
```

### Deals Closing This Month
```python
from datetime import datetime, timedelta

start = datetime.now().replace(day=1).strftime('%Y-%m-%d')
end = (datetime.now().replace(day=28) + timedelta(days=4)).replace(day=1) - timedelta(days=1)
end = end.strftime('%Y-%m-%d')

search = PublicObjectSearchRequest(
    filter_groups=[{
        'filters': [
            {'propertyName': 'closedate', 'operator': 'GTE', 'value': start},
            {'propertyName': 'closedate', 'operator': 'LTE', 'value': end}
        ]
    }],
    properties=['dealname', 'amount', 'dealstage', 'closedate']
)
results = client.crm.deals.search_api.do_search(search)
```

---

## Tickets

### List Tickets
```python
tickets = client.crm.tickets.basic_api.get_page(
    limit=100,
    properties=['subject', 'content', 'hs_ticket_priority', 'hs_pipeline_stage']
)
```

### Get Ticket by ID
```python
ticket = client.crm.tickets.basic_api.get_by_id(
    ticket_id='101',
    properties=['subject', 'content', 'hs_ticket_priority']
)
```

---

## Owners (Users)

### List Owners
```python
owners = client.crm.owners.owners_api.get_page()
for owner in owners.results:
    print(f'{owner.id}: {owner.first_name} {owner.last_name} ({owner.email})')
```

### Get Owner by ID
```python
owner = client.crm.owners.owners_api.get_by_id(owner_id='12345')
```

---

## Pipelines

### List Deal Pipelines
```python
pipelines = client.crm.pipelines.pipelines_api.get_all(object_type='deals')
for p in pipelines.results:
    print(f'{p.id}: {p.label}')
    for stage in p.stages:
        print(f'  - {stage.id}: {stage.label}')
```

---

## Associations

### Get Contacts for a Company
```python
associations = client.crm.companies.associations_api.get_all(
    company_id='456',
    to_object_type='contacts'
)
for assoc in associations.results:
    contact = client.crm.contacts.basic_api.get_by_id(assoc.id)
    print(contact.properties)
```

### Get Deals for a Company
```python
associations = client.crm.companies.associations_api.get_all(
    company_id='456',
    to_object_type='deals'
)
```

---

## Creating Records (Require Permission)

### Create Contact
```python
from hubspot.crm.contacts import SimplePublicObjectInputForCreate

contact = SimplePublicObjectInputForCreate(
    properties={
        'firstname': 'John',
        'lastname': 'Doe',
        'email': 'john@example.com'
    }
)
result = client.crm.contacts.basic_api.create(contact)
print(f'Created: {result.id}')
```

### Create Company
```python
from hubspot.crm.companies import SimplePublicObjectInputForCreate

company = SimplePublicObjectInputForCreate(
    properties={
        'name': 'Acme Corp',
        'domain': 'acme.com',
        'industry': 'Technology'
    }
)
result = client.crm.companies.basic_api.create(company)
```

### Create Deal
```python
from hubspot.crm.deals import SimplePublicObjectInputForCreate

deal = SimplePublicObjectInputForCreate(
    properties={
        'dealname': 'New Deal',
        'amount': '50000',
        'dealstage': 'appointmentscheduled',
        'pipeline': 'default'
    }
)
result = client.crm.deals.basic_api.create(deal)
```

---

## Updating Records (Require Permission)

### Update Contact
```python
from hubspot.crm.contacts import SimplePublicObjectInput

update = SimplePublicObjectInput(properties={'phone': '555-1234'})
client.crm.contacts.basic_api.update(contact_id='123', simple_public_object_input=update)
```

### Update Deal Stage
```python
from hubspot.crm.deals import SimplePublicObjectInput

update = SimplePublicObjectInput(properties={'dealstage': 'closedwon'})
client.crm.deals.basic_api.update(deal_id='789', simple_public_object_input=update)
```

---

## Search Operators

Available operators for filters:
- `EQ` - Equal
- `NEQ` - Not equal
- `LT` - Less than
- `LTE` - Less than or equal
- `GT` - Greater than
- `GTE` - Greater than or equal
- `CONTAINS_TOKEN` - Contains (for text)
- `NOT_CONTAINS_TOKEN` - Does not contain
- `HAS_PROPERTY` - Property exists
- `NOT_HAS_PROPERTY` - Property does not exist

---

## Error Handling

```python
from hubspot.crm.contacts.exceptions import ApiException

try:
    contact = client.crm.contacts.basic_api.get_by_id('invalid')
except ApiException as e:
    print(f'Error {e.status}: {e.reason}')
```

---

## Full Example

```python
import os
from hubspot import HubSpot

client = HubSpot(access_token=os.environ['HUBSPOT_ACCESS_TOKEN'])

# Get recent contacts
print("Recent Contacts:")
print("-" * 50)
contacts = client.crm.contacts.basic_api.get_page(
    limit=5,
    properties=['firstname', 'lastname', 'email', 'company']
)
for c in contacts.results:
    name = f"{c.properties.get('firstname', '')} {c.properties.get('lastname', '')}"
    email = c.properties.get('email', 'No email')
    print(f"- {name.strip()} ({email})")

# Get open deals
print("\nOpen Deals:")
print("-" * 50)
deals = client.crm.deals.basic_api.get_page(
    limit=5,
    properties=['dealname', 'amount', 'dealstage']
)
total = 0
for d in deals.results:
    amount = float(d.properties.get('amount') or 0)
    total += amount
    print(f"- {d.properties.get('dealname')}: ${amount:,.0f}")

print(f"\nTotal Pipeline: ${total:,.0f}")
```
