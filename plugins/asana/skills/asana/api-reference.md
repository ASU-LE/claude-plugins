# Asana API Reference - Python (asana)

Clean Python patterns using the official asana library.

## Setup

```bash
pip3 install asana
```

## Initialization

```python
import os
import asana

client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'
```

---

## Users

### Get Current User
```python
me = client.users.me()
print(f'{me["gid"]}: {me["name"]} ({me["email"]})')
```

### Get User by ID
```python
user = client.users.find_by_id('USER_GID')
```

### List Users in Workspace
```python
for user in client.users.find_all({'workspace': 'WORKSPACE_GID'}):
    print(f'{user["gid"]}: {user["name"]}')
```

---

## Workspaces

### List All Workspaces
```python
for ws in client.workspaces.find_all():
    print(f'{ws["gid"]}: {ws["name"]}')
```

### Get Workspace Details
```python
workspace = client.workspaces.find_by_id('WORKSPACE_GID')
```

---

## Projects

### List Projects in Workspace
```python
for proj in client.projects.find_all({'workspace': 'WORKSPACE_GID'}):
    print(f'{proj["gid"]}: {proj["name"]}')
```

### Get Project Details
```python
project = client.projects.find_by_id('PROJECT_GID', opt_fields='name,notes,owner.name,due_date')
```

### List Project Sections
```python
for section in client.sections.find_by_project('PROJECT_GID'):
    print(f'{section["gid"]}: {section["name"]}')
```

---

## Tasks

### List All Tasks in Project
```python
tasks = client.tasks.find_all({
    'project': 'PROJECT_GID',
    'opt_fields': 'name,completed,due_on,assignee.name'
})
for task in tasks:
    print(task['name'])
```

### List Incomplete Tasks Only
```python
tasks = client.tasks.find_all({
    'project': 'PROJECT_GID',
    'completed_since': 'now',  # Only incomplete tasks
    'opt_fields': 'name,due_on,assignee.name'
})
```

### List Tasks in Section
```python
tasks = client.tasks.find_all({
    'section': 'SECTION_GID',
    'opt_fields': 'name,completed,due_on'
})
```

### Get Task Details
```python
task = client.tasks.find_by_id('TASK_GID', opt_fields='name,notes,completed,due_on,assignee.name,projects.name,tags.name')
```

### Search Tasks in Workspace
```python
tasks = client.tasks.search_tasks_for_workspace('WORKSPACE_GID', {
    'text': 'search term',
    'opt_fields': 'name,completed,due_on'
})
```

### Tasks Assigned to User
```python
me = client.users.me()
tasks = client.tasks.find_all({
    'assignee': me['gid'],
    'workspace': 'WORKSPACE_GID',
    'completed_since': 'now',
    'opt_fields': 'name,due_on,projects.name'
})
```

### Tasks Due This Week
```python
from datetime import datetime, timedelta

today = datetime.now().strftime('%Y-%m-%d')
week_later = (datetime.now() + timedelta(days=7)).strftime('%Y-%m-%d')

tasks = client.tasks.search_tasks_for_workspace('WORKSPACE_GID', {
    'due_on.after': today,
    'due_on.before': week_later,
    'completed': False,
    'opt_fields': 'name,due_on,assignee.name'
})
```

---

## Tasks - Writing (Require Permission)

### Create Task
```python
task = client.tasks.create_task({
    'name': 'New Task',
    'projects': ['PROJECT_GID'],
    'notes': 'Task description',
    'due_on': '2024-01-20'
})
print(f'Created: {task["gid"]}')
```

### Create Task with Assignee
```python
task = client.tasks.create_task({
    'name': 'New Task',
    'projects': ['PROJECT_GID'],
    'assignee': 'USER_GID'
})
```

### Update Task
```python
client.tasks.update_task('TASK_GID', {
    'completed': True
})
```

### Update Task Fields
```python
client.tasks.update_task('TASK_GID', {
    'name': 'Updated Name',
    'notes': 'Updated notes',
    'due_on': '2024-01-25'
})
```

### Add Task to Project
```python
client.tasks.add_project_for_task('TASK_GID', {
    'project': 'PROJECT_GID'
})
```

### Move Task to Section
```python
client.sections.add_task_for_section('SECTION_GID', {
    'task': 'TASK_GID'
})
```

### Delete Task
```python
client.tasks.delete_task('TASK_GID')
```

---

## Subtasks

### List Subtasks
```python
for subtask in client.tasks.subtasks('TASK_GID'):
    print(f'{subtask["gid"]}: {subtask["name"]}')
```

### Create Subtask
```python
subtask = client.tasks.create_subtask_for_task('PARENT_TASK_GID', {
    'name': 'Subtask Name'
})
```

---

## Comments (Stories)

### List Comments on Task
```python
stories = client.stories.find_by_task('TASK_GID')
for story in stories:
    if story.get('type') == 'comment':
        print(f'{story["created_by"]["name"]}: {story["text"]}')
```

### Add Comment
```python
story = client.stories.create_story_for_task('TASK_GID', {
    'text': 'This is a comment'
})
```

---

## Tags

### List Tags in Workspace
```python
for tag in client.tags.find_all({'workspace': 'WORKSPACE_GID'}):
    print(f'{tag["gid"]}: {tag["name"]}')
```

### Add Tag to Task
```python
client.tasks.add_tag_for_task('TASK_GID', {
    'tag': 'TAG_GID'
})
```

---

## Useful opt_fields

Common fields to request:

### For Tasks
```python
opt_fields='name,completed,due_on,due_at,assignee.name,projects.name,tags.name,notes,created_at,modified_at'
```

### For Projects
```python
opt_fields='name,notes,owner.name,due_date,start_on,created_at,archived'
```

### For Users
```python
opt_fields='name,email,photo.image_128x128'
```

---

## Error Handling

```python
from asana.error import InvalidRequestError, NotFoundError, ForbiddenError

try:
    task = client.tasks.find_by_id('invalid_id')
except NotFoundError:
    print("Task not found")
except ForbiddenError:
    print("No permission to access this task")
except InvalidRequestError as e:
    print(f"Invalid request: {e}")
```

---

## Rate Limits

- Asana: ~150 requests per minute
- The client handles rate limiting automatically with retries
- No manual handling needed

---

## Full Example

```python
import os
import asana

# Initialize
client = asana.Client.access_token(os.environ['ASANA_ACCESS_TOKEN'])
client.options['client_name'] = 'Claude Assistant'

# Get current user
me = client.users.me()
print(f"Connected as: {me['name']}")

# Get first workspace
workspaces = list(client.workspaces.find_all())
workspace_gid = workspaces[0]['gid']

# List my incomplete tasks
print("\nMy Open Tasks:")
print("-" * 50)
tasks = client.tasks.find_all({
    'assignee': me['gid'],
    'workspace': workspace_gid,
    'completed_since': 'now',
    'opt_fields': 'name,due_on,projects.name'
})

for task in tasks:
    due = task.get('due_on', 'No date')
    projects = ', '.join([p['name'] for p in task.get('projects', [])])
    print(f"- {task['name']}")
    print(f"  Due: {due} | Project: {projects}")
```
