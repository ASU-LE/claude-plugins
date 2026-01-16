# Pages Operations

Pages are wiki-style content pages in Canvas courses.

## Listing Pages

### With MCP
```
Use: mcp__canvas-mcp__list_course_pages
Parameters:
  - course_id: "12345"
  - published: true (only show published pages)
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/pages?per_page=50" | \
  jq -r '.[] | "\(.url): \(.title)"'
```

## Getting Page Content

### With MCP
```
Use: mcp__canvas-mcp__get_course_page
Parameters:
  - course_id: "12345"
  - url_or_id: "syllabus" (the page URL slug)
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/pages/PAGE_URL" | \
  jq -r '.body'
```

## Getting Front Page

Often the syllabus or course overview.

### With MCP
```
Use: mcp__canvas-mcp__get_course_front_page
Parameters:
  - course_id: "12345"
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/front_page" | \
  jq -r '.body'
```

## Displaying Page Content

Page body is HTML. When displaying:

1. Extract key information
2. Summarize if very long
3. Preserve structure (headers, lists)
4. Don't dump raw HTML to user

```
Page: Course Syllabus

Instructor: Dr. Smith
Office Hours: Mon/Wed 2-4pm
Email: smith@university.edu

Course Overview:
This course covers fundamentals of...

Grading:
- Homework: 30%
- Midterm: 30%
- Final: 40%

[Full page has more content - ask if you want details on a specific section]
```

## Common User Requests

| User says | Action |
|-----------|--------|
| "Show me the syllabus" | Get front page or search for "syllabus" |
| "What are the office hours?" | Search pages for office hours info |
| "Show the grading policy" | Find page with grading info |
| "List all pages" | List page titles |

## Finding Specific Information

If user asks for specific info (office hours, grading, etc.):

1. First check the front page
2. Then search page titles for keywords
3. Read matching pages
4. Extract the relevant section

```bash
# Search pages by title
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/pages?search_term=syllabus"
```
