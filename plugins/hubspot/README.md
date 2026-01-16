# HubSpot Plugin for Claude Code

Access HubSpot CRM data directly from Claude Code.

## Features

- List and search contacts, companies, deals, tickets
- View deal pipelines and stages
- Check associations between records
- Privacy-focused with read-only defaults

## Prerequisites

- Python 3.7+
- HubSpot Private App Access Token

## Setup

1. Install the library:
   ```bash
   pip3 install hubspot-api-client
   ```

2. Create a Private App in HubSpot:
   - Settings → Integrations → Private Apps
   - Create app with required scopes
   - Copy the access token

3. Set environment variable:
   ```bash
   export HUBSPOT_ACCESS_TOKEN="pat-na1-xxxxxxxx"
   ```

## Usage

Once configured, just mention HubSpot in your conversation:
- "Show me recent contacts in HubSpot"
- "List open deals"
- "Find companies in the Technology industry"

## Scopes Needed

Minimum read scopes:
- `crm.objects.contacts.read`
- `crm.objects.companies.read`
- `crm.objects.deals.read`
- `crm.objects.owners.read`

For write operations (optional):
- `crm.objects.contacts.write`
- `crm.objects.companies.write`
- `crm.objects.deals.write`
