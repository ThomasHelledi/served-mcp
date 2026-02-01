# Context Tools

Tools for establishing and retrieving context about the user and workspace.

---

## GetUserContext

**Always call this first.** Returns user profile, workspaces, and permissions.

### Parameters

None required.

### Response

```json
{
  "user": {
    "id": 123,
    "name": "Thomas Helledi",
    "email": "thomas@example.com"
  },
  "workspaces": [
    {
      "id": 1,
      "name": "Served",
      "slug": "served",
      "role": "admin"
    }
  ],
  "currentWorkspace": {
    "id": 1,
    "name": "Served",
    "slug": "served"
  },
  "permissions": ["projects", "tasks", "timetracking", "customers"]
}
```

### Example Usage

```
User: "What can I access?"
AI: Calls GetUserContext
AI: "You're logged in as Thomas Helledi with access to the Served workspace.
     You have permissions for projects, tasks, time tracking, and customers."
```

---

## GetTenantContext

Get detailed information about the current workspace including settings, categories, and team.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tenantId` | int | No | Specific tenant ID (uses current if omitted) |

### Response

```json
{
  "tenant": {
    "id": 1,
    "name": "Served",
    "slug": "served",
    "settings": {
      "currency": "DKK",
      "timezone": "Europe/Copenhagen",
      "weekStart": "monday"
    }
  },
  "categories": [
    { "id": 1, "name": "Development" },
    { "id": 2, "name": "Design" },
    { "id": 3, "name": "Marketing" }
  ],
  "team": [
    { "id": 1, "name": "Thomas Helledi", "role": "admin" },
    { "id": 2, "name": "Atlas", "role": "member" }
  ],
  "projectCount": 15,
  "activeProjectCount": 8
}
```

### Example Usage

```
User: "What categories do we have for projects?"
AI: Calls GetTenantContext
AI: "Your workspace has 3 categories: Development, Design, and Marketing."
```

---

## GetProjectContext

Get comprehensive project information including tasks, team, and recent activity.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | Yes | The project ID |

### Response

```json
{
  "project": {
    "id": 123,
    "name": "Website Redesign",
    "status": "active",
    "progress": 0.65,
    "budgetHours": 200,
    "usedHours": 130
  },
  "tasks": {
    "total": 24,
    "completed": 16,
    "inProgress": 5,
    "todo": 3,
    "recentTasks": [
      { "id": 1, "name": "Homepage design", "status": "completed" },
      { "id": 2, "name": "API integration", "status": "in_progress" }
    ]
  },
  "team": [
    { "id": 1, "name": "Thomas", "hoursLogged": 80 },
    { "id": 2, "name": "Developer", "hoursLogged": 50 }
  ],
  "recentActivity": [
    { "type": "task_completed", "task": "Homepage design", "by": "Thomas", "at": "2026-01-31T10:00:00Z" },
    { "type": "time_logged", "hours": 2, "task": "API integration", "by": "Developer", "at": "2026-01-31T09:00:00Z" }
  ]
}
```

### Example Usage

```
User: "Give me an overview of the Website project"
AI: Calls GetProjectContext with projectId
AI: "The Website Redesign project is 65% complete.
     16 of 24 tasks are done, with 5 in progress.
     You've used 130 of 200 budgeted hours.
     Recent activity: Thomas completed the Homepage design task."
```

---

## Best Practices

1. **Call GetUserContext at session start** - establishes who you are and what you can access
2. **Use GetTenantContext for workspace-wide info** - categories, team members, settings
3. **Use GetProjectContext for project deep-dives** - combines project, tasks, and activity in one call
4. **Cache context within a session** - don't call repeatedly for the same info
