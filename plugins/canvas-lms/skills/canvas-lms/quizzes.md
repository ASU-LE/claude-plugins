# Quizzes Operations

## Listing Quizzes

### With MCP
```
Use: mcp__canvas-mcp__list_quizzes
Parameters:
  - course_id: "12345"
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/quizzes?per_page=50" | \
  jq -r '.[] | "\(.title) - Due: \(.due_at // "No due date") - \(.time_limit // "No") min limit"'
```

## Quiz Types

| Type | Description |
|------|-------------|
| `practice_quiz` | Ungraded practice |
| `assignment` | Graded quiz |
| `graded_survey` | Survey with points |
| `survey` | Ungraded survey |

## Getting Quiz Details

### With MCP
```
Use: mcp__canvas-mcp__get_quiz
Parameters:
  - course_id: "12345"
  - quiz_id: "67890"
```

### With Curl
```bash
curl -s -H "Authorization: Bearer $CANVAS_API_TOKEN" \
  "$CANVAS_BASE_URL/api/v1/courses/COURSE_ID/quizzes/QUIZ_ID"
```

## Displaying Quizzes

Format with important timing info:

```
Quizzes in This Course:

1. Quiz 1: Introduction Concepts
   Type: Graded quiz
   Due: Friday, Jan 19 at 11:59 PM
   Time limit: 30 minutes
   Attempts: 2 allowed
   Points: 50

2. Practice Quiz: Chapter 2
   Type: Practice (not graded)
   Due: No due date
   Time limit: None
   Attempts: Unlimited

3. Midterm Exam
   Type: Graded quiz
   Due: Feb 15 at 2:00 PM
   Time limit: 90 minutes
   Attempts: 1
   Points: 100
```

## Important Quiz Settings to Show

When displaying quiz details, highlight:

- **Time limit**: How long they have once started
- **Attempts allowed**: How many tries
- **Due date**: When it must be completed
- **Available dates**: When they can access it
- **Points possible**: How much it's worth
- **Question count**: Number of questions

## Common User Requests

| User says | Action |
|-----------|--------|
| "What quizzes do I have?" | List all quizzes |
| "When is the midterm?" | Search quizzes for "midterm" |
| "How long is quiz 3?" | Get quiz details, show time limit |
| "How many attempts for [quiz]?" | Get quiz, show allowed_attempts |

## Privacy Note

NEVER show quiz questions or answers through this skill.

If user asks for quiz questions:
> I can show you quiz details like due dates and time limits, but I can't display quiz questions or answers. You'll need to take the quiz directly in Canvas.

This protects academic integrity.
