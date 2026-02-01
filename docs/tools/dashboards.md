# Dashboard & Reporting Tools

Tools for creating and managing dashboards, widgets, and data queries.

---

## Dashboard Tools

### GetDashboards

List all dashboards.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `scope` | string | No | Personal, Workspace, or Tenant |

#### Response

```json
{
  "dashboards": [
    {
      "id": 1,
      "name": "Project Overview",
      "description": "Key project metrics",
      "widgetCount": 6,
      "isDefault": true,
      "scope": "Workspace"
    }
  ],
  "total": 5
}
```

#### Example Usage

```
User: "Show my dashboards"
AI: Calls GetDashboards
AI: "You have 5 dashboards:

     Workspace Dashboards:
     - Project Overview (default) - 6 widgets
     - Team Performance - 4 widgets

     Personal Dashboards:
     - My Tasks - 3 widgets
     - Time Tracking - 2 widgets
     - Custom Report - 5 widgets"
```

---

### GetDashboardDetails

Get dashboard with all widgets.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `dashboardId` | int | Yes | Dashboard ID |

#### Response

```json
{
  "dashboard": {
    "id": 1,
    "name": "Project Overview",
    "description": "Key project metrics",
    "theme": "light",
    "refreshIntervalSeconds": 300,
    "widgets": [
      {
        "id": 101,
        "type": "metric",
        "title": "Active Projects",
        "gridX": 0,
        "gridY": 0,
        "gridWidth": 2,
        "gridHeight": 1,
        "config": {
          "datasourceId": 1,
          "query": "projects.count(status='active')"
        }
      }
    ]
  }
}
```

---

### CreateDashboard

Create a new dashboard.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Dashboard name |
| `description` | string | No | Description |
| `theme` | string | No | light or dark |
| `refreshIntervalSeconds` | int | No | Auto-refresh interval |
| `scope` | string | No | Personal, Workspace, Tenant |

#### Example Usage

```
User: "Create a sales dashboard"
AI: Calls CreateDashboard with name="Sales Dashboard", scope="Workspace"
AI: "Created dashboard 'Sales Dashboard' (ID: 6). Ready to add widgets."
```

---

### SetDefaultDashboard

Set a dashboard as the default for its scope.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `dashboardId` | int | Yes | Dashboard ID |

---

### DuplicateDashboard

Duplicate an existing dashboard.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `dashboardId` | int | Yes | Dashboard to copy |
| `newName` | string | Yes | Name for the copy |

---

## Widget Tools

### GetWidgets

Get widgets for a dashboard.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `dashboardId` | int | Yes | Dashboard ID |

#### Response

```json
{
  "widgets": [
    {
      "id": 101,
      "type": "metric",
      "title": "Active Projects",
      "position": { "x": 0, "y": 0 },
      "size": { "width": 2, "height": 1 }
    },
    {
      "id": 102,
      "type": "chart",
      "title": "Revenue Trend",
      "position": { "x": 2, "y": 0 },
      "size": { "width": 4, "height": 2 }
    }
  ]
}
```

---

### CreateWidget

Add a widget to a dashboard.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `dashboardId` | int | Yes | Target dashboard |
| `type` | string | Yes | Widget type (see below) |
| `title` | string | Yes | Widget title |
| `gridX` | int | No | X position (default: 0) |
| `gridY` | int | No | Y position (default: 0) |
| `gridWidth` | int | No | Width in grid units |
| `gridHeight` | int | No | Height in grid units |
| `config` | object | No | Widget-specific config |

#### Widget Types

| Type | Description |
|------|-------------|
| `metric` | Single number with trend |
| `chart` | Line, bar, area charts |
| `pie` | Pie/donut charts |
| `table` | Data table |
| `list` | Simple list |
| `map` | Geographic map |
| `calendar` | Calendar heatmap |
| `progress` | Progress bar/gauge |
| `text` | Rich text/markdown |

#### Example Usage

```
User: "Add a metric widget showing total revenue"
AI: Calls CreateWidget with type="metric", title="Total Revenue", config={...}
AI: "Added 'Total Revenue' metric widget to your dashboard"
```

---

### UpdateWidget

Update widget settings.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `widgetId` | int | Yes | Widget ID |
| `title` | string | No | New title |
| `config` | object | No | New configuration |

---

### UpdateWidgetLayout

Move/resize widgets.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `widgetId` | int | Yes | Widget ID |
| `gridX` | int | No | New X position |
| `gridY` | int | No | New Y position |
| `gridWidth` | int | No | New width |
| `gridHeight` | int | No | New height |

---

### DeleteWidget

Remove a widget.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `widgetId` | int | Yes | Widget ID |

---

## Datasource Tools

### GetDatasourceEntities

List available data entities.

#### Response

```json
{
  "entities": [
    {
      "name": "projects",
      "displayName": "Projects",
      "category": "Project Management",
      "fields": ["id", "name", "status", "budget", "customerId"]
    },
    {
      "name": "tasks",
      "displayName": "Tasks",
      "category": "Project Management",
      "fields": ["id", "name", "status", "projectId", "assigneeId"]
    }
  ]
}
```

---

### GetEntitySchema

Get schema for a data entity.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `entityName` | string | Yes | Entity name |

#### Response

```json
{
  "entity": "projects",
  "fields": [
    { "name": "id", "type": "int", "filterable": true },
    { "name": "name", "type": "string", "filterable": true },
    { "name": "status", "type": "enum", "values": ["active", "completed", "archived"] },
    { "name": "budget", "type": "decimal", "aggregatable": true },
    { "name": "createdAt", "type": "datetime", "filterable": true }
  ],
  "relationships": [
    { "name": "customer", "entity": "customers", "type": "many-to-one" },
    { "name": "tasks", "entity": "tasks", "type": "one-to-many" }
  ]
}
```

---

### ExecuteDatasourceQuery

Execute a data query.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `entityName` | string | Yes | Data entity |
| `select` | array | No | Fields to return |
| `filter` | object | No | Filter conditions |
| `groupBy` | array | No | Group by fields |
| `orderBy` | array | No | Sort order |
| `limit` | int | No | Max results |

#### Example Usage

```
User: "How many active projects per customer?"
AI: Calls ExecuteDatasourceQuery with:
    entityName="projects",
    filter={status:"active"},
    groupBy=["customerId"],
    select=["customerId", "count(*)"]
AI: "Active projects by customer:
     - Acme Corp: 5 projects
     - TechStart: 3 projects
     - FinTech: 2 projects"
```

---

### PreviewDatasourceQuery

Preview query results without saving.

#### Parameters

Same as ExecuteDatasourceQuery, but with:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `limit` | int | No | Preview limit (default: 10) |

---

### ValidateDatasourceQuery

Validate query syntax without executing.

#### Parameters

Same as ExecuteDatasourceQuery.

#### Response

```json
{
  "valid": true,
  "estimatedRows": 150,
  "warnings": []
}
```

---

## Best Practices

1. **Use appropriate widget sizes** - Metrics: 2x1, Charts: 4x2, Tables: 6x3
2. **Set refresh intervals** - Real-time data: 30s, Reports: 300s
3. **Validate queries** - Use ValidateDatasourceQuery before saving
4. **Scope dashboards correctly** - Personal for drafts, Workspace for shared
