# Project Tools

Tools for managing projects in your workspace.

---

## GetProjects

List all projects with optional filtering.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `status` | string | No | Filter: active, completed, archived |
| `customerId` | int | No | Filter by customer |
| `categoryId` | int | No | Filter by category |
| `take` | int | No | Max results (default: 50) |
| `skip` | int | No | Offset for pagination |

### Response

```json
{
  "projects": [
    {
      "id": 1,
      "name": "Website Redesign",
      "status": "active",
      "customerName": "Acme Corp",
      "budgetHours": 200,
      "usedHours": 130,
      "progress": 0.65,
      "dueDate": "2026-03-15"
    }
  ],
  "total": 15
}
```

### Example Usage

```
User: "Show me all active projects"
AI: Calls GetProjects with status="active"
AI: "You have 8 active projects:
     1. Website Redesign (Acme Corp) - 65% complete
     2. Mobile App (TechStart) - 30% complete
     ..."
```

---

## GetProjectDetails

Get detailed information about a specific project.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | Yes | The project ID |

### Response

```json
{
  "id": 1,
  "name": "Website Redesign",
  "description": "Complete website overhaul with new design system",
  "status": "active",
  "customer": {
    "id": 10,
    "name": "Acme Corp"
  },
  "category": {
    "id": 1,
    "name": "Development"
  },
  "budgetHours": 200,
  "usedHours": 130,
  "startDate": "2026-01-01",
  "dueDate": "2026-03-15",
  "assignedEmployees": [
    { "id": 1, "name": "Thomas" },
    { "id": 2, "name": "Developer" }
  ],
  "taskSummary": {
    "total": 24,
    "completed": 16,
    "inProgress": 5,
    "todo": 3
  }
}
```

---

## CreateProject

Create a new project.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Project name |
| `description` | string | No | Project description |
| `customerId` | int | No | Associated customer |
| `categoryId` | int | No | Project category |
| `budgetHours` | decimal | No | Hour budget |
| `startDate` | date | No | Start date |
| `dueDate` | date | No | Due date |

### Response

```json
{
  "success": true,
  "project": {
    "id": 25,
    "name": "New Project",
    "status": "active"
  },
  "message": "Project created successfully"
}
```

### Example Usage

```
User: "Create a new project called API Integration for Acme Corp with 80 hours budget"
AI: Calls CreateProject with name="API Integration", customerId=10, budgetHours=80
AI: "Created project 'API Integration' (ID: 25) for Acme Corp with an 80-hour budget."
```

---

## UpdateProject

Update an existing project.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | Yes | Project to update |
| `name` | string | No | New name |
| `description` | string | No | New description |
| `status` | string | No | New status |
| `budgetHours` | decimal | No | New budget |
| `dueDate` | date | No | New due date |

### Example Usage

```
User: "Extend the Website project deadline to April 15th and add 50 hours to the budget"
AI: Calls UpdateProject with projectId=1, dueDate="2026-04-15", budgetHours=250
AI: "Updated Website Redesign: new deadline April 15th, budget increased to 250 hours."
```

---

## DeleteProject

Delete a project.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | Yes | Project to delete |

### Response

```json
{
  "success": true,
  "message": "Project deleted successfully"
}
```

> **Warning:** This permanently deletes the project and all associated tasks and time entries.

### Example Usage

```
User: "Delete the old Test Project"
AI: "Are you sure you want to delete 'Test Project'? This will also delete all tasks and time entries."
User: "Yes, delete it"
AI: Calls DeleteProject with projectId=99
AI: "Deleted project 'Test Project' and all associated data."
```

---

## Best Practices

1. **Use GetProjects with filters** rather than getting all and filtering client-side
2. **Always confirm before DeleteProject** - it's irreversible
3. **Set budgetHours** for financial tracking and health analysis
4. **Use categories** for organization and reporting
