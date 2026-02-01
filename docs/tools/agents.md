# Agent Coordination Tools

Tools for AI agent management, coordination, and collaboration.

---

## Agent Discovery

### GetActiveAgents

List all active AI agents.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `includeIdle` | bool | No | Include idle agents |

#### Response

```json
{
  "agents": [
    {
      "id": "atlas-main-001",
      "name": "Atlas",
      "type": "developer",
      "status": "active",
      "currentTask": "Implementing dashboard widgets",
      "workingFiles": ["src/components/Widget.tsx"],
      "startedAt": "2026-02-01T09:00:00Z",
      "tokenUsage": 45000,
      "budget": 100000
    }
  ],
  "summary": {
    "total": 3,
    "active": 2,
    "idle": 1
  }
}
```

#### Example Usage

```
User: "What agents are running?"
AI: Calls GetActiveAgents
AI: "3 agents active:

     1. Atlas (developer) - Working on dashboard widgets
        Files: Widget.tsx
        Tokens: 45K / 100K budget

     2. CodeReviewer (reviewer) - Idle
        Last activity: 5 min ago

     3. TestRunner (tester) - Running tests
        Current: API integration tests"
```

---

### GetAgentDetails

Get detailed agent information.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | Yes | Agent ID |

#### Response

```json
{
  "agent": {
    "id": "atlas-main-001",
    "name": "Atlas",
    "type": "developer",
    "status": "active",
    "currentTask": "Implementing dashboard widgets",
    "workingFiles": [
      "src/components/Widget.tsx",
      "src/services/DashboardService.ts"
    ],
    "recentActions": [
      { "action": "file_edit", "file": "Widget.tsx", "time": "10:30:00" },
      { "action": "file_read", "file": "types.ts", "time": "10:29:45" }
    ],
    "metrics": {
      "tokenUsage": 45000,
      "budget": 100000,
      "filesModified": 3,
      "linesChanged": 156
    }
  }
}
```

---

### GetAgentContext

Get what an agent knows/has access to.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | Yes | Agent ID |

#### Response

```json
{
  "context": {
    "projectId": 123,
    "taskId": 456,
    "workingDirectory": "/src/components",
    "openFiles": ["Widget.tsx", "types.ts"],
    "recentSearches": ["dashboard widget", "chart component"],
    "conversationSummary": "Implementing metric widgets for dashboard"
  }
}
```

---

## Coordination Tools

### GetCoordinationInfo

Get coordination status between agents.

#### Response

```json
{
  "coordination": {
    "activeConflicts": 0,
    "pendingHandoffs": 1,
    "sharedResources": [
      {
        "file": "src/services/DashboardService.ts",
        "lockedBy": "atlas-main-001",
        "since": "2026-02-01T10:25:00Z"
      }
    ]
  }
}
```

---

### CoordinateWithAgent

Request coordination with another agent.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `targetAgentId` | string | Yes | Agent to coordinate with |
| `action` | string | Yes | request_file, handoff, notify |
| `resource` | string | No | File or resource name |
| `message` | string | No | Message to agent |

#### Example Usage

```
User: "Ask the other agent to release DashboardService.ts"
AI: Calls CoordinateWithAgent with action="request_file", resource="DashboardService.ts"
AI: "Sent file release request to Atlas. Waiting for response..."
```

---

### GetFilesInUse

Check which files are being used by agents.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `path` | string | No | Filter by path pattern |

#### Response

```json
{
  "filesInUse": [
    {
      "file": "src/components/Widget.tsx",
      "agent": "atlas-main-001",
      "mode": "write",
      "since": "2026-02-01T10:25:00Z"
    }
  ]
}
```

---

### DetectConflicts

Check for potential conflicts.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `files` | array | Yes | Files to check |

#### Response

```json
{
  "conflicts": [
    {
      "file": "src/services/DashboardService.ts",
      "conflictType": "concurrent_edit",
      "agents": ["atlas-main-001", "feature-agent-002"],
      "severity": "high"
    }
  ],
  "safe": false
}
```

---

## Agent Control

### SendAgentTask

Assign a task to an agent.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | Yes | Target agent |
| `task` | string | Yes | Task description |
| `priority` | string | No | low, normal, high |
| `context` | object | No | Additional context |

---

### PauseAgent

Pause an agent's execution.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | Yes | Agent to pause |
| `reason` | string | No | Pause reason |

---

### ResumeAgent

Resume a paused agent.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | Yes | Agent to resume |

---

### KillAgent

Terminate an agent.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `agentId` | string | Yes | Agent to kill |
| `force` | bool | No | Force immediate termination |

---

### KillStaleAgents

Kill agents that have been idle too long.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `idleMinutes` | int | No | Idle threshold (default: 30) |

---

### KillOverBudgetAgents

Kill agents exceeding their token budget.

---

## Agent Communication

### AtlasAsk

Ask Atlas (primary agent) a question.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `question` | string | Yes | Question to ask |
| `context` | object | No | Additional context |

#### Example Usage

```
User: "Ask Atlas about the current project status"
AI: Calls AtlasAsk with question="What's the current project status?"
AI: "Atlas responds: Currently working on dashboard widgets.
     3 components complete, 2 in progress.
     Estimated completion: 2 hours."
```

---

### AtlasNotify

Send a notification to Atlas.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `message` | string | Yes | Notification message |
| `priority` | string | No | low, normal, high, urgent |

---

### AtlasGetPendingRequests

Get pending requests from other agents.

#### Response

```json
{
  "requests": [
    {
      "id": "req-001",
      "from": "test-runner-001",
      "type": "file_review",
      "message": "Please review Widget.tsx changes",
      "createdAt": "2026-02-01T10:30:00Z"
    }
  ]
}
```

---

### AtlasRespond

Respond to a pending request.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `requestId` | string | Yes | Request ID |
| `response` | string | Yes | Response message |
| `action` | string | No | approve, reject, defer |

---

## Best Practices

1. **Check for conflicts** - Always use DetectConflicts before editing shared files
2. **Release files** - Release file locks when done editing
3. **Monitor budgets** - Track token usage to avoid budget overruns
4. **Coordinate handoffs** - Use proper handoff when passing work between agents
5. **Kill stale agents** - Regularly clean up idle agents
