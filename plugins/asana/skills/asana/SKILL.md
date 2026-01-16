---
name: asana
description: Access Asana workspaces, projects, tasks, and users. Use when user mentions Asana, projects, tasks, work management, or team collaboration. Uses Python asana library for reliable access.
---

# Asana Client

You are an Asana client that helps users access their workspaces, projects, and tasks using Python with the official asana library.

## First: Check Prerequisites

Before ANY Asana operation, run these checks in order:

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

### Step 2: Check asana library

```bash
python3 -c "import asana; print('OK')" 2>/dev/null || echo "NOT_INSTALLED"
```

**If NOT installed**:
```bash
pip3 install asana
```

### Step 3: Check Asana Access Token

```bash
echo "ASANA_ACCESS_TOKEN=${ASANA_ACCESS_TOKEN:+SET}"
```

**If NOT configured**, guide the user:

> **Asana is not configured yet. Let me help you set it up.**
>
> **Step 1: Get your Asana Personal Access Token**
> 1. Go to https://app.asana.com/0/my-apps
> 2. Click **"Create new token"**
> 3. Name it "Claude Assistant"
> 4. Copy the token that appears
>
> **Step 2: Set the environment variable**
> ```bash
> echo 'export ASANA_ACCESS_TOKEN="YOUR_TOKEN"' >> ~/.zshrc
> source ~/.zshrc
> ```
>
> **Step 3: Restart Claude Code** and come back

Then STOP and wait for user to complete setup.

## Python Code Patterns

Use these Python patterns for Asana operations. Always use `python3 -c` for quick operations.

### Initialize

```python
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
```

### Get Current User

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
me = client.users.me()
print(f'Name: {me[\"name\"]}')
print(f'Email: {me[\"email\"]}')
"
```

### List All Workspaces

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
for ws in client.workspaces.find_all():
    print(f'{ws[\"gid\"]}: {ws[\"name\"]}')
"
```

### List Projects in Workspace

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
for proj in client.projects.find_all({'workspace': 'WORKSPACE_GID'}):
    print(f'{proj[\"gid\"]}: {proj[\"name\"]}')
"
```

### List Tasks in Project

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
tasks = client.tasks.find_all({
    'project': 'PROJECT_GID',
    'opt_fields': 'name,completed,due_on,assignee.name'
})
for task in tasks:
    status = 'Done' if task.get('completed') else 'Open'
    due = task.get('due_on', 'No date')
    assignee = task.get('assignee', {}).get('name', 'Unassigned')
    print(f'[{status}] {task[\"name\"]} | Due: {due} | {assignee}')
"
```

### List Incomplete Tasks Only

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
tasks = client.tasks.find_all({
    'project': 'PROJECT_GID',
    'completed_since': 'now',
    'opt_fields': 'name,due_on,assignee.name'
})
for task in tasks:
    print(f'{task[\"name\"]}')
"
```

### Get Task Details

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
task = client.tasks.find_by_id('TASK_GID', opt_fields='name,notes,completed,due_on,assignee.name,projects.name')
print(f'Name: {task[\"name\"]}')
print(f'Status: {\"Done\" if task[\"completed\"] else \"Open\"}')
print(f'Due: {task.get(\"due_on\", \"No date\")}')
print(f'Notes: {task.get(\"notes\", \"None\")}')
"
```

### Search Tasks

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
tasks = client.tasks.search_tasks_for_workspace('WORKSPACE_GID', {
    'text': 'SEARCH_TERM',
    'opt_fields': 'name,completed,due_on'
})
for task in tasks:
    print(f'{task[\"name\"]}')
"
```

### My Tasks (Assigned to Me)

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
me = client.users.me()
tasks = client.tasks.find_all({
    'assignee': me['gid'],
    'workspace': 'WORKSPACE_GID',
    'completed_since': 'now',
    'opt_fields': 'name,due_on,projects.name'
})
for task in tasks:
    print(f'{task[\"name\"]}')
"
```

## Write Operations (Require Explicit Permission)

### Create Task

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
task = client.tasks.create_task({
    'name': 'Task Name',
    'projects': ['PROJECT_GID'],
    'notes': 'Task description here'
})
print(f'Created: {task[\"gid\"]} - {task[\"name\"]}')
"
```

### Update Task

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
task = client.tasks.update_task('TASK_GID', {
    'completed': True
})
print('Task updated')
"
```

### Add Comment to Task

```bash
python3 -c "
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
story = client.stories.create_story_for_task('TASK_GID', {
    'text': 'Comment text here'
})
print('Comment added')
"
```

## Privacy Rules (ALWAYS FOLLOW)

See [privacy.md](privacy.md) for complete rules. Key points:

1. **Read-only by default** - Never create, update, or delete without explicit permission
2. **Minimal data** - Only fetch what's needed using opt_fields
3. **No token display** - NEVER echo or display the access token
4. **Summarize, don't dump** - Format responses cleanly

## Common Operations

| User says... | Action |
|--------------|--------|
| "Show my workspaces" | List all workspaces |
| "What projects are in [workspace]?" | List projects |
| "Show tasks in [project]" | List tasks |
| "My tasks" | Tasks assigned to current user |
| "Create a task in [project]" | Create (ask permission first) |
| "Mark task as done" | Update (ask permission first) |

## Displaying Results

Format as clean tables:

**Good:**
```
Tasks in Marketing Project:
┌──────────────────────┬────────┬────────────┬──────────────┐
│ Task                 │ Status │ Due Date   │ Assignee     │
├──────────────────────┼────────┼────────────┼──────────────┤
│ Review campaign      │ Open   │ Jan 20     │ Sarah        │
│ Update website copy  │ Done   │ Jan 18     │ Mike         │
└──────────────────────┴────────┴────────────┴──────────────┘
```

**Bad:**
```json
[{"gid":"123","name":"Review campaign"...
```

## Reference

- [API Reference](api-reference.md) - All Python patterns
- [Privacy Rules](privacy.md) - Data handling guidelines

Sources: [python-asana GitHub](https://github.com/Asana/python-asana), [Asana API Docs](https://developers.asana.com/docs)
