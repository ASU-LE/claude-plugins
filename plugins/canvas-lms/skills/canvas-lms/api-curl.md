# Canvas API - Curl Patterns

Use these patterns when MCP tools are not available. Always use environment variables for credentials.

## Base Pattern

```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/ENDPOINT"
```

## Courses

### List my courses
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses?enrollment_state=active&per_page=50"
```

### Get specific course
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID"
```

## Modules

### List modules in a course
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/modules?per_page=50"
```

### List items in a module
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/modules/MODULE_ID/items?per_page=50"
```

## Pages

### List pages in a course
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/pages?per_page=50"
```

### Get specific page content
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/pages/PAGE_URL"
```

### Get front page (syllabus often here)
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/front_page"
```

## Assignments

### List assignments
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/assignments?per_page=50&order_by=due_at"
```

### Get specific assignment
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/assignments/ASSIGNMENT_ID"
```

### List upcoming assignments (due in future)
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/assignments?bucket=upcoming&per_page=20"
```

## Quizzes

### List quizzes
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/quizzes?per_page=50"
```

### Get specific quiz
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/quizzes/QUIZ_ID"
```

## User Info

### Get current user (self)
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/users/self"
```

### Get user's todo items
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/users/self/todo"
```

### Get upcoming events
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/users/self/upcoming_events"
```

## Response Parsing

Always pipe through `jq` for clean output:

```bash
# List course names only
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses?enrollment_state=active" | \
  jq -r '.[] | "\(.id): \(.name)"'

# Get assignment names and due dates
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/assignments" | \
  jq -r '.[] | "\(.name) - Due: \(.due_at // "No due date")"'
```

## Pagination

Canvas uses Link headers for pagination. For simple cases, use `per_page=100` (max).

For complete data:
```bash
# This gets first page - check Link header for next page
curl -s -I -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses" | grep -i "^link:"
```

## Error Handling

Check response before parsing:
```bash
response=$(curl -s -w "\n%{http_code}" -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses")
http_code=$(echo "$response" | tail -n1)
body=$(echo "$response" | sed '$d')

if [ "$http_code" -ne 200 ]; then
  echo "Error: HTTP $http_code"
else
  echo "$body" | jq '.'
fi
```
