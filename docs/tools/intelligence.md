# AI Intelligence Tools

AI-powered analysis and suggestions for your projects.

---

## AnalyzeProjectHealth

Get a comprehensive health analysis of a project with score, risks, and recommendations.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | Yes | Project to analyze |

### Response

```json
{
  "projectId": 1,
  "projectName": "Website Redesign",
  "healthScore": 72,
  "status": "needs_attention",
  "metrics": {
    "progressScore": 85,
    "budgetScore": 65,
    "velocityScore": 70,
    "taskCompletionRate": 0.67
  },
  "risks": [
    {
      "type": "budget_overrun",
      "severity": "medium",
      "description": "Project is 65% through budget but only 55% complete",
      "recommendation": "Review remaining tasks and prioritize or request budget extension"
    },
    {
      "type": "deadline_risk",
      "severity": "low",
      "description": "Current velocity suggests completion 5 days after deadline",
      "recommendation": "Consider adding resources or reducing scope"
    }
  ],
  "recommendations": [
    "Focus on completing the 3 high-priority blocked tasks",
    "Review and update task estimates based on actual time spent",
    "Schedule a sprint planning session to realign priorities"
  ],
  "trends": {
    "velocity": "decreasing",
    "taskCompletion": "stable",
    "budgetBurn": "above_target"
  }
}
```

### Health Score Ranges

| Score | Status | Description |
|-------|--------|-------------|
| 80-100 | `healthy` | On track |
| 60-79 | `needs_attention` | Some concerns |
| 40-59 | `at_risk` | Significant issues |
| 0-39 | `critical` | Immediate action needed |

### Example Usage

```
User: "How healthy is the Website project?"
AI: Calls AnalyzeProjectHealth with projectId=1
AI: "The Website Redesign project has a health score of 72/100 (needs attention).

     Key concerns:
     - Budget: 65% used but only 55% complete
     - Deadline: Current pace suggests 5-day delay

     Recommendations:
     1. Focus on the 3 blocked high-priority tasks
     2. Review and update remaining estimates
     3. Schedule sprint planning to realign priorities"
```

---

## SuggestTaskDecomposition

Get AI suggestions for breaking down a complex task.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskId` | int | No | Existing task to decompose |
| `taskName` | string | No | New task name to decompose |
| `taskDescription` | string | No | Description for context |
| `projectId` | int | No | Project context |

### Response

```json
{
  "originalTask": "Build user authentication system",
  "suggestedSubtasks": [
    {
      "name": "Design authentication flow",
      "description": "Create wireframes for login, signup, password reset",
      "estimatedHours": 4,
      "priority": "high",
      "order": 1
    },
    {
      "name": "Implement database schema",
      "description": "Users table, sessions, password reset tokens",
      "estimatedHours": 2,
      "priority": "high",
      "order": 2
    },
    {
      "name": "Build login/signup forms",
      "description": "Frontend forms with validation",
      "estimatedHours": 6,
      "priority": "medium",
      "order": 3
    },
    {
      "name": "Implement backend auth API",
      "description": "JWT tokens, session management, password hashing",
      "estimatedHours": 8,
      "priority": "high",
      "order": 4
    },
    {
      "name": "Add OAuth providers",
      "description": "Google, GitHub OAuth integration",
      "estimatedHours": 6,
      "priority": "low",
      "order": 5
    },
    {
      "name": "Write authentication tests",
      "description": "Unit and integration tests",
      "estimatedHours": 4,
      "priority": "medium",
      "order": 6
    }
  ],
  "totalEstimatedHours": 30,
  "reasoning": "Authentication is a critical security feature requiring careful design, implementation, and testing. Decomposed into logical phases following security best practices."
}
```

### Example Usage

```
User: "Break down the authentication task into subtasks"
AI: Calls SuggestTaskDecomposition with taskId=5
AI: "Here's a suggested breakdown for 'Build user authentication' (30 hours total):

     1. Design authentication flow (4h) - HIGH
     2. Implement database schema (2h) - HIGH
     3. Build login/signup forms (6h) - MEDIUM
     4. Implement backend auth API (8h) - HIGH
     5. Add OAuth providers (6h) - LOW
     6. Write authentication tests (4h) - MEDIUM

     Want me to create these subtasks?"
```

---

## EstimateEffort

Get AI-powered effort estimation based on project history.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskName` | string | Yes | Task to estimate |
| `taskDescription` | string | No | Additional context |
| `projectId` | int | No | Project for context |

### Response

```json
{
  "taskName": "Implement search functionality",
  "estimate": {
    "hours": 12,
    "confidence": 0.75,
    "range": {
      "optimistic": 8,
      "realistic": 12,
      "pessimistic": 20
    }
  },
  "similarTasks": [
    {
      "name": "Implement filter system",
      "projectName": "E-commerce App",
      "estimatedHours": 10,
      "actualHours": 14
    },
    {
      "name": "Add search to dashboard",
      "projectName": "Admin Portal",
      "estimatedHours": 8,
      "actualHours": 10
    }
  ],
  "factors": [
    "Similar search implementations averaged 12 hours",
    "Your team tends to underestimate by 20%",
    "Consider adding buffer for edge cases"
  ]
}
```

### Example Usage

```
User: "How long will the search feature take?"
AI: Calls EstimateEffort with taskName="Implement search functionality"
AI: "Based on similar tasks in your history, I estimate 12 hours (75% confidence).

     Range: 8-20 hours (optimistic to pessimistic)

     Similar completed tasks:
     - Filter system: estimated 10h, actual 14h
     - Dashboard search: estimated 8h, actual 10h

     Note: Your team typically underestimates by ~20%, so buffer accordingly."
```

---

## FindSimilarProjects

Find projects similar to a given project for reference.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | int | No | Reference project |
| `projectName` | string | No | Search by name |
| `take` | int | No | Max results (default: 5) |

### Response

```json
{
  "referenceProject": "Website Redesign",
  "similarProjects": [
    {
      "id": 5,
      "name": "Marketing Site Rebuild",
      "similarity": 0.85,
      "customer": "Tech Corp",
      "status": "completed",
      "actualHours": 180,
      "budgetHours": 160,
      "duration": "3 months",
      "insights": "Completed 12% over budget, mainly due to scope creep in design phase"
    }
  ]
}
```

---

## Best Practices

1. **Use AnalyzeProjectHealth regularly** - catch issues early
2. **Decompose tasks over 8 hours** - improves tracking and estimation
3. **Review EstimateEffort similar tasks** - learn from history
4. **Act on recommendations** - the AI insights are actionable
