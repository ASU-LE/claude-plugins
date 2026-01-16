# Courses Operations

## Listing Courses

### With MCP
```
Use: mcp__canvas-mcp__list_courses
Parameters:
  - enrollment_state: "active" (default for current courses)
  - include: ["term"] for semester info
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses?enrollment_state=active&per_page=50" | \
  jq -r '.[] | "[\(.id)] \(.name) (\(.course_code))"'
```

## Displaying Courses to User

Format as a clean list:

```
Your Active Courses:
1. Introduction to Psychology (PSY101) - ID: 12345
2. Calculus II (MATH201) - ID: 12346
3. English Composition (ENG102) - ID: 12347
```

Always include the course ID - users will need it for follow-up requests.

## Getting Course Details

### With MCP
```
Use: mcp__canvas-mcp__get_course
Parameters:
  - course_id: "12345"
  - include: ["syllabus_body", "term", "teachers"]
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID?include[]=syllabus_body&include[]=term"
```

## Common User Requests

| User says | Action |
|-----------|--------|
| "What courses do I have?" | List active courses |
| "Show my fall courses" | List courses, filter by term if possible |
| "Tell me about [course name]" | Search courses by name, get details |
| "What's the syllabus for [course]?" | Get course with syllabus_body include |

## Finding Course by Name

Users often say course names, not IDs. Process:
1. List all courses
2. Find matching name (fuzzy match if needed)
3. Use that course's ID for further operations

```bash
# Search for course by name
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses?enrollment_state=active" | \
  jq -r '.[] | select(.name | test("SEARCH_TERM"; "i")) | "\(.id): \(.name)"'
```
