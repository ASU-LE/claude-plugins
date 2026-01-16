# Assignments Operations

## Listing Assignments

### With MCP
```
Use: mcp__canvas-mcp__list_assignments
Parameters:
  - course_id: "12345"
  - order_by: "due_at"
  - bucket: "upcoming" (for future assignments)
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/assignments?order_by=due_at&per_page=50" | \
  jq -r '.[] | "\(.name) - Due: \(.due_at // "No due date") - \(.points_possible) pts"'
```

## Assignment Buckets

Use buckets to filter assignments:

| Bucket | Description |
|--------|-------------|
| `upcoming` | Due in the future |
| `past` | Due date has passed |
| `overdue` | Past due, no submission |
| `undated` | No due date set |
| `ungraded` | Submitted but not graded |
| `unsubmitted` | Not yet submitted |

```bash
# Get upcoming assignments
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/assignments?bucket=upcoming"
```

## Getting Assignment Details

### With MCP
```
Use: mcp__canvas-mcp__get_assignment
Parameters:
  - course_id: "12345"
  - assignment_id: "67890"
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/assignments/ASSIGNMENT_ID"
```

## Displaying Assignments

Format clearly with due dates prominent:

```
Upcoming Assignments:

1. Homework 3: Data Analysis
   Due: Friday, Jan 19 at 11:59 PM
   Points: 100
   Submission: Online upload (PDF)

2. Reading Response 4
   Due: Sunday, Jan 21 at 11:59 PM
   Points: 20
   Submission: Text entry

3. Group Project Proposal
   Due: Monday, Jan 22 at 5:00 PM
   Points: 50
   Submission: Online upload
   Note: Group assignment
```

## What's Due Soon

Get assignments due in the next week:

```bash
# Get upcoming assignments, filter by date client-side
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/assignments?bucket=upcoming&order_by=due_at&per_page=10"
```

## Cross-Course Assignments

To show all assignments across all courses:

```bash
# Get user's todo items (includes assignments)
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/users/self/todo" | \
  jq -r '.[] | "\(.assignment.name) - \(.context_name) - Due: \(.assignment.due_at)"'
```

## Common User Requests

| User says | Action |
|-----------|--------|
| "What's due this week?" | List upcoming assignments, filter by date |
| "Show all assignments" | List all assignments for course |
| "Tell me about [assignment]" | Get specific assignment details |
| "What haven't I submitted?" | Use bucket=unsubmitted |
| "What's overdue?" | Use bucket=overdue |

## Submission Types

Explain what submission type means:

| Type | User action needed |
|------|-------------------|
| `online_text_entry` | Type response in Canvas |
| `online_upload` | Upload a file |
| `online_url` | Submit a link |
| `media_recording` | Record audio/video |
| `on_paper` | Physical submission |
| `none` | No submission needed |
