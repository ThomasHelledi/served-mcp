# MCP Tools Reference

Complete reference for all 180+ Served MCP tools.

---

## Tool Categories

| Category | Tools | Documentation |
|----------|-------|---------------|
| **Context** | GetUserContext, GetTenantContext, GetProjectContext | [Context Tools](./context.md) |
| **Projects** | GetProjects, CreateProject, UpdateProject, DeleteProject | [Project Tools](./projects.md) |
| **Tasks** | GetTasks, CreateTask, UpdateTask, DeleteTask, CreateTasksBulk | [Task Tools](./tasks.md) |
| **Time** | CreateTimeRegistration, GetTimeRegistrations, SuggestTimeEntries | [Time Tools](./time.md) |
| **Intelligence** | AnalyzeProjectHealth, SuggestTaskDecomposition, EstimateEffort | [AI Tools](./intelligence.md) |
| **DevOps** | GetRepositories, GetPullRequests, GetPipelineRuns, GetJobLog | [DevOps Tools](./devops.md) |
| **Customers** | GetCustomers, GetCustomerDetails, CreateCustomer | [Customer Tools](./customers.md) |
| **Dashboards** | GetDashboards, CreateWidget, ExecuteDatasourceQuery | [Dashboard Tools](./dashboards.md) |
| **Agents** | GetActiveAgents, CoordinateWithAgent, AtlasAsk | [Agent Tools](./agents.md) |
| **Infrastructure** | GetClusterHealth, GetKubernetesHealth, GetInfrastructureResources | [Infrastructure Tools](./infrastructure.md) |
| **Workflows** | GetWorkflows, ExecuteWorkflow, GetWorkflowRuns | Workflow Tools |
| **Integrations** | GetConfiguredIntegrations, TestIntegrationConnection | Integration Tools |
| **Files** | served_file_find, served_file_stats, served_file_tree | File Tools |
| **Analytics** | GetToolUsageStats, GetTopCliCommands, GetAgentSessions | Analytics Tools |

---

## Quick Reference

### Always Start Here

```
GetUserContext
```

This tool returns your user profile, available workspaces, and permissions. **Call this first** in any session.

### Common Workflows

| Goal | Tools to Use |
|------|--------------|
| See my work | `GetUserContext` → `GetProjects` → `GetTasks` |
| Create work | `CreateProject` → `CreateTask` or `CreateTasksBulk` |
| Log time | `GetTasks` → `CreateTimeRegistration` |
| Get insights | `AnalyzeProjectHealth` or `EstimateEffort` |

---

## Tool Naming Convention

| Prefix | Operation |
|--------|-----------|
| `Get*` | Read/retrieve data |
| `Create*` | Create new resource |
| `Update*` | Modify existing resource |
| `Delete*` | Remove resource |
| `Analyze*` | AI-powered analysis |
| `Suggest*` | AI-powered suggestions |
| `Estimate*` | AI-powered estimation |
| `Find*` | Search/discovery |

---

## Response Format

All tools return structured JSON responses:

### Success Response

```json
{
  "success": true,
  "data": { ... },
  "message": "Operation completed successfully"
}
```

### Error Response

```json
{
  "success": false,
  "error": "Error description",
  "code": "ERROR_CODE"
}
```

---

## Pagination

Tools that return lists support pagination:

| Parameter | Type | Description |
|-----------|------|-------------|
| `take` | int | Max items to return (default: 50) |
| `skip` | int | Items to skip (for pagination) |

Example:
```
"Get the next 50 projects after the first 100"
→ GetProjects with take=50, skip=100
```

---

## Filtering

Most list tools support filtering:

| Parameter | Description |
|-----------|-------------|
| `status` | Filter by status |
| `projectId` | Filter by project |
| `assigneeId` | Filter by assigned user |
| `customerId` | Filter by customer |
| `startDate` / `endDate` | Date range |

---

## Best Practices

1. **Always call GetUserContext first** - establishes session context
2. **Use bulk operations** when creating multiple items
3. **Filter results** to reduce data transfer
4. **Use AI tools** for insights, not just CRUD
5. **Check success flag** in responses before proceeding
