# DevOps Tools

Repository, pull request, and pipeline management tools.

---

## Repository Tools

### GetDevOpsRepositories

List all connected repositories.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `activeOnly` | bool | No | Only active repos (default: true) |

#### Response

```json
{
  "repositories": [
    {
      "id": 1,
      "name": "ServedApp",
      "provider": "forge",
      "url": "git@git.unifiedhq.ai:served/servedapp.git",
      "defaultBranch": "master",
      "isActive": true,
      "lastSynced": "2026-02-01T10:30:00Z"
    }
  ],
  "total": 5
}
```

#### Example Usage

```
User: "What repos are connected?"
AI: Calls GetDevOpsRepositories
AI: "You have 5 connected repositories:
     1. ServedApp (Forge) - master
     2. served-sdk (GitHub) - main
     3. served-mcp (GitHub) - main
     4. unifiedhq-web (Forge) - master
     5. mobile-app (Forge) - develop"
```

---

### GetDevOpsRepository

Get repository details.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | int | Yes | Repository ID |

#### Response

```json
{
  "repository": {
    "id": 1,
    "name": "ServedApp",
    "provider": "forge",
    "url": "git@git.unifiedhq.ai:served/servedapp.git",
    "defaultBranch": "master",
    "isActive": true,
    "createdAt": "2024-01-01T00:00:00Z",
    "lastSynced": "2026-02-01T10:30:00Z",
    "stats": {
      "openPRs": 3,
      "mergedPRsThisMonth": 45,
      "pipelineSuccessRate": 0.92
    }
  }
}
```

---

### ConnectRepository

Connect a new repository.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Repository name |
| `provider` | string | Yes | github, gitlab, forge, azure, bitbucket |
| `url` | string | Yes | Repository URL |
| `defaultBranch` | string | No | Default branch (default: main) |

#### Example Usage

```
User: "Connect my new repo https://github.com/company/new-project"
AI: Calls ConnectRepository with provider="github", url="...", name="new-project"
AI: "Connected repository 'new-project' from GitHub. Default branch: main"
```

---

### DisconnectRepository

Disconnect a repository.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | int | Yes | Repository ID |

---

## Pull Request Tools

### GetPullRequests

List pull requests across all repositories.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `state` | string | No | Open, Merged, Closed |
| `repositoryId` | int | No | Filter by repository |
| `limit` | int | No | Max results (default: 50) |

#### Response

```json
{
  "pullRequests": [
    {
      "id": 123,
      "number": 456,
      "repositoryName": "ServedApp",
      "title": "feat: Add dashboard widgets",
      "state": "Open",
      "sourceBranch": "feature/widgets",
      "targetBranch": "master",
      "author": "thomas",
      "createdAt": "2026-02-01T09:00:00Z",
      "pipelineStatus": "Success"
    }
  ],
  "summary": {
    "open": 5,
    "merged": 45,
    "closed": 3
  }
}
```

#### Example Usage

```
User: "Show open PRs"
AI: Calls GetPullRequests with state="Open"
AI: "5 open pull requests:

     ServedApp:
     - #456: feat: Add dashboard widgets (thomas) - Pipeline: Success
     - #455: fix: Time registration bug (atlas) - Pipeline: Running

     served-sdk:
     - #89: docs: Update API reference (atlas) - Pipeline: Pending"
```

---

### GetTaskPullRequests

Get pull requests linked to a task.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskId` | int | Yes | Task ID |

#### Response

```json
{
  "pullRequests": [
    {
      "id": 123,
      "number": 456,
      "title": "feat: Implement task #789",
      "state": "Open",
      "pipelineStatus": "Success"
    }
  ],
  "task": {
    "id": 789,
    "name": "Add user authentication"
  }
}
```

---

### GetAgentSessionPullRequests

Get PRs created by an AI agent session.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `sessionId` | int | Yes | Agent session ID |

#### Response

```json
{
  "pullRequests": [
    {
      "id": 123,
      "title": "feat: Automated implementation",
      "state": "Open",
      "agentType": "Atlas"
    }
  ],
  "session": {
    "id": 456,
    "agentName": "Atlas",
    "startedAt": "2026-02-01T09:00:00Z"
  }
}
```

---

### LinkPullRequestToTask

Link a PR to a task for tracking.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pullRequestId` | int | Yes | Pull request ID |
| `taskId` | int | Yes | Task ID |

---

## Pipeline Tools

### GetPipelineRuns

Get pipeline runs for a PR or repository.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pullRequestId` | int | No | Filter by PR |
| `repositoryId` | int | No | Filter by repository |
| `limit` | int | No | Max results (default: 50) |

#### Response

```json
{
  "runs": [
    {
      "id": 789,
      "name": "CI/CD Pipeline",
      "status": "Success",
      "startedAt": "2026-02-01T10:00:00Z",
      "finishedAt": "2026-02-01T10:05:32Z",
      "duration": "5m 32s",
      "jobs": [
        { "name": "build", "status": "Success", "duration": "2m 15s" },
        { "name": "test", "status": "Success", "duration": "1m 45s" },
        { "name": "deploy", "status": "Success", "duration": "1m 32s" }
      ]
    }
  ]
}
```

#### Example Usage

```
User: "Show pipeline history for ServedApp"
AI: Calls GetPipelineRuns with repositoryId=1
AI: "Last 5 pipeline runs for ServedApp:

     #789 - Success (5m 32s) - 10:05 today
     #788 - Success (4m 58s) - 09:30 today
     #787 - Failed (3m 12s) - Yesterday
     #786 - Success (5m 01s) - Yesterday
     #785 - Success (4m 45s) - Yesterday

     Success rate: 80% (last 5 runs)"
```

---

### GetLatestPipelineRun

Get the most recent pipeline run.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pullRequestId` | int | Yes | Pull request ID |

#### Response

```json
{
  "run": {
    "id": 789,
    "status": "Success",
    "startedAt": "2026-02-01T10:00:00Z",
    "finishedAt": "2026-02-01T10:05:32Z",
    "duration": "5m 32s",
    "url": "https://git.unifiedhq.ai/served/servedapp/-/pipelines/789"
  }
}
```

---

### GetPipelineJobs

Get jobs within a pipeline run.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `pipelineId` | int | Yes | Pipeline run ID |

#### Response

```json
{
  "jobs": [
    {
      "id": 1001,
      "name": "build",
      "stage": "build",
      "status": "Success",
      "duration": "2m 15s",
      "startedAt": "2026-02-01T10:00:00Z"
    },
    {
      "id": 1002,
      "name": "test",
      "stage": "test",
      "status": "Success",
      "duration": "1m 45s"
    }
  ]
}
```

---

### GetJobLog

Get logs for a specific job.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `jobId` | int | Yes | Job ID |
| `lines` | int | No | Last N lines (default: 100) |

#### Response

```json
{
  "log": "[10:00:01] Starting build...\n[10:00:15] Compiling...\n[10:02:15] Build complete",
  "jobName": "build",
  "status": "Success"
}
```

---

### RetryJob

Retry a failed job.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `jobId` | int | Yes | Job ID to retry |

---

### CancelJob

Cancel a running job.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `jobId` | int | Yes | Job ID to cancel |

---

## Best Practices

1. **Link PRs to tasks** - Always link PRs for traceability
2. **Monitor pipeline health** - Track success rates regularly
3. **Review agent PRs** - Agent-created PRs need human review
4. **Use branch protection** - Require passing pipelines before merge
