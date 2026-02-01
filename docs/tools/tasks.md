# Task Tools

Tools for managing tasks within projects.

---

## GetTasks

Get tasks with optional filtering.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | No | Filter by project |
| `assigneeId` | int | No | Filter by assigned employee |
| `status` | string | No | Filter: todo, in_progress, review, completed |
| `priority` | string | No | Filter: low, medium, high, critical |
| `includeCompleted` | bool | No | Include completed tasks (default: true) |
| `take` | int | No | Max results (default: 50) |

### Response

```json
{
  "tasks": [
    {
      "id": 1,
      "name": "Implement login page",
      "status": "in_progress",
      "priority": "high",
      "projectName": "Website Redesign",
      "assigneeName": "Thomas",
      "estimatedHours": 8,
      "loggedHours": 4,
      "dueDate": "2026-02-05"
    }
  ],
  "total": 24
}
```

### Example Usage

```
User: "What are my open tasks?"
AI: Calls GetTasks with assigneeId=currentUser, includeCompleted=false
AI: "You have 5 open tasks:
     1. Implement login page (HIGH) - 4/8 hours logged
     2. API documentation (MEDIUM) - not started
     ..."
```

---

## GetTaskDetails

Get detailed information about a specific task.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskId` | int | Yes | The task ID |

### Response

```json
{
  "id": 1,
  "name": "Implement login page",
  "description": "Create login form with email/password and OAuth support",
  "status": "in_progress",
  "priority": "high",
  "project": {
    "id": 1,
    "name": "Website Redesign"
  },
  "assignee": {
    "id": 1,
    "name": "Thomas"
  },
  "estimatedHours": 8,
  "loggedHours": 4,
  "dueDate": "2026-02-05",
  "subtasks": [
    { "id": 2, "name": "Email/password form", "status": "completed" },
    { "id": 3, "name": "OAuth integration", "status": "in_progress" }
  ],
  "timeEntries": [
    { "date": "2026-02-01", "hours": 2, "description": "Form layout" },
    { "date": "2026-02-02", "hours": 2, "description": "Validation logic" }
  ]
}
```

---

## CreateTask

Create a new task.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | Yes | Parent project |
| `name` | string | Yes | Task name |
| `description` | string | No | Task description |
| `assigneeId` | int | No | Assigned employee |
| `priority` | string | No | Priority level |
| `estimatedHours` | decimal | No | Hour estimate |
| `dueDate` | date | No | Due date |
| `parentTaskId` | int | No | Parent task (for subtasks) |

### Example Usage

```
User: "Create a task for reviewing the homepage design, assign to me, due Friday"
AI: Calls CreateTask with projectId=1, name="Review homepage design", assigneeId=1, dueDate="2026-02-07"
AI: "Created task 'Review homepage design' assigned to you, due Friday."
```

---

## CreateTasksBulk

Create multiple tasks at once.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | Yes | Parent project |
| `tasks` | array | Yes | Array of task objects |

### Example Usage

```
User: "Create tasks for the API project: Authentication, CRUD endpoints, Documentation, Testing"
AI: Calls CreateTasksBulk with projectId and 4 tasks
AI: "Created 4 tasks in the API project:
     - Authentication (ID: 50)
     - CRUD endpoints (ID: 51)
     - Documentation (ID: 52)
     - Testing (ID: 53)"
```

---

## UpdateTask

Update an existing task.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskId` | int | Yes | Task to update |
| `name` | string | No | New name |
| `description` | string | No | New description |
| `status` | string | No | New status |
| `priority` | string | No | New priority |
| `assigneeId` | int | No | New assignee |
| `estimatedHours` | decimal | No | New estimate |
| `dueDate` | date | No | New due date |

### Example Usage

```
User: "Mark the login task as completed"
AI: Calls UpdateTask with taskId=1, status="completed"
AI: "Marked 'Implement login page' as completed."
```

---

## DeleteTask

Delete a task.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskId` | int | Yes | Task to delete |

> **Warning:** This also deletes all subtasks and time entries associated with the task.

---

## Task Statuses

| Status | Description |
|--------|-------------|
| `todo` | Not started |
| `in_progress` | Currently being worked on |
| `review` | Pending review |
| `completed` | Done |
| `blocked` | Blocked by dependency |

## Task Priorities

| Priority | Description |
|----------|-------------|
| `low` | Nice to have |
| `medium` | Normal priority (default) |
| `high` | Important |
| `critical` | Urgent/blocking |

---

## Best Practices

1. **Use CreateTasksBulk** when creating multiple related tasks
2. **Set estimatedHours** for accurate project tracking
3. **Use subtasks** for complex tasks that need breakdown
4. **Update status regularly** for accurate project health
