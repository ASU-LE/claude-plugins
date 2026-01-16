# Modules Operations

Modules are the main organizational structure in Canvas courses.

## Listing Modules

### With MCP
```
Use: mcp__canvas-mcp__list_modules
Parameters:
  - course_id: "12345"
  - include: ["items"] to get module items inline
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/modules?per_page=50" | \
  jq -r '.[] | "[\(.id)] \(.name) - \(.items_count) items"'
```

## Displaying Modules

Format as a structured list:

```
Course Modules:

1. Week 1: Introduction (5 items)
   - Published, available now

2. Week 2: Core Concepts (8 items)
   - Published, unlocks Jan 15

3. Week 3: Advanced Topics (6 items)
   - Not published yet
```

## Getting Module Items

### With MCP
```
Use: mcp__canvas-mcp__list_module_items
Parameters:
  - course_id: "12345"
  - module_id: "67890"
  - include: ["content_details"]
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/modules/MODULE_ID/items?per_page=50" | \
  jq -r '.[] | "  - [\(.type)] \(.title)"'
```

## Module Item Types

| Type | Description |
|------|-------------|
| Page | Wiki page content |
| Assignment | Graded assignment |
| Quiz | Quiz or survey |
| File | Downloadable file |
| Discussion | Discussion topic |
| ExternalUrl | Link to external site |
| ExternalTool | LTI tool launch |
| SubHeader | Section divider (no content) |

## Common User Requests

| User says | Action |
|-----------|--------|
| "What's in this course?" | List all modules |
| "Show me week 3" | Find module by name, list its items |
| "What do I need to do?" | List modules with completion status |
| "What's available now?" | List published, unlocked modules |

## Showing Module Content

When user asks about a specific module:

1. List the module's items
2. For each item, show:
   - Type (Page, Assignment, etc.)
   - Title
   - Due date (if assignment/quiz)
   - Completion status (if available)

```
Module: Week 2 - Core Concepts

Items:
1. [Page] Reading: Chapter 3
2. [Assignment] Homework 2 - Due: Jan 20
3. [Quiz] Quiz 2 - Due: Jan 22
4. [Discussion] Weekly Discussion
5. [File] Lecture Slides
```
