# Time Tracking Tools

Tools for logging and analyzing time entries.

---

## CreateTimeRegistration

Log a time entry.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskId` | int | Yes* | Task to log time on |
| `projectId` | int | Yes* | Project (if no task) |
| `date` | date | Yes | Date of work |
| `hours` | decimal | Yes | Hours worked |
| `description` | string | No | Work description |
| `billable` | bool | No | Is billable (default: true) |

*Either `taskId` or `projectId` is required.

### Response

```json
{
  "success": true,
  "timeEntry": {
    "id": 789,
    "date": "2026-02-01",
    "hours": 2.5,
    "taskId": 456,
    "taskName": "API Integration",
    "projectName": "Website Redesign"
  },
  "message": "Logged 2.5 hours"
}
```

### Example Usage

```
User: "Log 2 hours on the API task"
AI: Calls CreateTimeRegistration with taskId=456, hours=2, date=today
AI: "Logged 2 hours on 'API Integration'. Total time on this task: 6h."
```

---

## GetTimeRegistrations

Get time entries with optional filtering.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `startDate` | date | No | Filter from date |
| `endDate` | date | No | Filter to date |
| `projectId` | int | No | Filter by project |
| `taskId` | int | No | Filter by task |
| `employeeId` | int | No | Filter by employee |
| `take` | int | No | Max results (default: 50) |

### Response

```json
{
  "timeEntries": [
    {
      "id": 789,
      "date": "2026-02-01",
      "hours": 2.5,
      "description": "Working on API integration",
      "taskName": "API Integration",
      "projectName": "Website Redesign",
      "employeeName": "Thomas",
      "billable": true
    }
  ],
  "total": 15,
  "totalHours": 42.5
}
```

### Example Usage

```
User: "How many hours did I log this week?"
AI: Calls GetTimeRegistrations with startDate=Monday, endDate=today
AI: "You logged 32 hours this week across 5 projects."
```

---

## SuggestTimeEntries

AI-powered suggestions for time registration based on activity.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `date` | date | No | Date to suggest for (default: today) |
| `employeeId` | int | No | Employee to suggest for |

### Response

```json
{
  "date": "2026-02-01",
  "suggestions": [
    {
      "taskId": 456,
      "taskName": "API Integration",
      "projectName": "Website Redesign",
      "suggestedHours": 4,
      "confidence": 0.85,
      "reason": "You typically work on this task on Mondays"
    },
    {
      "taskId": 789,
      "taskName": "Code Review",
      "projectName": "Mobile App",
      "suggestedHours": 2,
      "confidence": 0.70,
      "reason": "PR #123 was merged today"
    }
  ],
  "totalSuggestedHours": 6
}
```

### Example Usage

```
User: "What should I log for today?"
AI: Calls SuggestTimeEntries
AI: "Based on your activity, I suggest:
     - API Integration: 4h (you typically work on this Mondays)
     - Code Review: 2h (PR #123 was merged today)

     Want me to log these entries?"
```

---

## AnalyzeTimePatterns

Analyze time tracking patterns for insights.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `employeeId` | int | No | Employee to analyze |
| `periodDays` | int | No | Days to analyze (default: 30) |

### Response

```json
{
  "period": {
    "start": "2026-01-02",
    "end": "2026-02-01",
    "totalDays": 30
  },
  "summary": {
    "totalHours": 160,
    "billableHours": 140,
    "billablePercentage": 0.875,
    "averageHoursPerDay": 8,
    "daysWithEntries": 20
  },
  "byProject": [
    {
      "projectName": "Website Redesign",
      "hours": 80,
      "percentage": 0.50
    },
    {
      "projectName": "Mobile App",
      "hours": 60,
      "percentage": 0.375
    }
  ],
  "byDayOfWeek": {
    "monday": 8.5,
    "tuesday": 8.2,
    "wednesday": 8.0,
    "thursday": 7.8,
    "friday": 7.5
  },
  "insights": [
    "You log most hours on Mondays (avg 8.5h)",
    "87.5% of your time is billable (above 80% target)",
    "Website Redesign takes 50% of your time"
  ]
}
```

### Example Usage

```
User: "Analyze my time tracking for the last month"
AI: Calls AnalyzeTimePatterns with periodDays=30
AI: "Last 30 days time analysis:

     **Summary:**
     - Total: 160 hours
     - Billable: 140h (87.5%)
     - Average: 8h/day

     **Top Projects:**
     - Website Redesign: 80h (50%)
     - Mobile App: 60h (37.5%)

     **Insights:**
     - Most productive on Mondays
     - Billable rate above 80% target"
```

---

## Best Practices

1. **Log daily** - More accurate than weekly summaries
2. **Use descriptions** - Helps with invoicing and reports
3. **Check suggestions** - AI learns your patterns over time
4. **Review patterns** - Monthly analysis reveals optimization opportunities
