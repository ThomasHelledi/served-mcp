# Common Workflows

Real-world examples of using Served MCP tools effectively.

---

## Daily Standup

### Get Overview of Current Work

```
User: "What's my status for today's standup?"

AI workflow:
1. GetUserContext → identify current user
2. GetTasks → filter by assignee, includeCompleted=false
3. GetTimeRegistrations → last 24 hours

AI response:
"Here's your standup summary:

**Yesterday:**
- Completed: Login page frontend (4h logged)
- Progressed: API integration (2h logged)

**Today:**
- Continue: API integration (4h remaining)
- Start: Unit tests for auth module

**Blockers:**
- Waiting on design assets for profile page"
```

---

## Sprint Planning

### Analyze and Plan Sprint

```
User: "Help me plan the next sprint for the Website project"

AI workflow:
1. GetProjectContext → projectId for Website
2. AnalyzeProjectHealth → current state
3. GetTasks → status=todo or status=blocked
4. EstimateEffort → for unclear tasks

AI response:
"Sprint Planning for Website Redesign:

**Project Health: 72/100**
- 35% of budget remaining
- 8 tasks still to complete

**Recommended Sprint Backlog (40h capacity):**
1. Profile page design (8h) - HIGH
2. Settings API (6h) - HIGH
3. Notification system (12h) - MEDIUM
4. Mobile responsive fixes (8h) - MEDIUM

**Deferred to Next Sprint:**
- Admin dashboard (16h)
- Analytics integration (20h)

**Risks:**
- Profile page blocked on design assets
- Consider breaking down notification system"
```

---

## Time Logging

### Quick Time Entry

```
User: "Log 2 hours on the API task"

AI workflow:
1. GetTasks → search for "API" in current context
2. CreateTimeRegistration → taskId, hours=2, date=today

AI response:
"Logged 2 hours on 'API integration' (Task #45).
Total time on this task: 6 hours.
Task estimate: 8 hours (75% complete by time)."
```

### End of Day Logging

```
User: "I worked on the login page for 3 hours and meetings for 1 hour"

AI workflow:
1. GetTasks → find "login page" task
2. CreateTimeRegistration → login page, 3h
3. CreateTimeRegistration → internal/meetings category, 1h

AI response:
"Logged today's time:
- Login page implementation: 3h
- Internal meetings: 1h

Daily total: 4 hours
Weekly total: 18 hours"
```

---

## Project Setup

### Create New Project with Structure

```
User: "Create a new project for mobile app development for TechStart, 200 hour budget, due in 3 months"

AI workflow:
1. CreateProject → name, customerId, budgetHours, dueDate
2. SuggestTaskDecomposition → "Mobile app development"
3. CreateTasksBulk → create suggested tasks

AI response:
"Created project 'Mobile App' for TechStart:
- Budget: 200 hours
- Deadline: May 1, 2026

Created 8 initial tasks:
1. Requirements & wireframes (20h)
2. UI/UX design (30h)
3. Core app structure (16h)
4. Feature: Authentication (24h)
5. Feature: Main screens (40h)
6. API integration (30h)
7. Testing & QA (24h)
8. Deployment & launch (16h)

Project ready to start!"
```

---

## Project Review

### Weekly Health Check

```
User: "How are all my active projects doing?"

AI workflow:
1. GetProjects → status=active
2. AnalyzeProjectHealth → for each project

AI response:
"Weekly Project Health Summary:

| Project | Health | Budget | Deadline |
|---------|--------|--------|----------|
| Website Redesign | 72% ⚠️ | 65% used | On track |
| Mobile App | 88% ✅ | 40% used | On track |
| API Integration | 45% 🔴 | 90% used | At risk |

**Needs Attention:**
- API Integration is critically over budget
- Recommend scope review or budget extension

**Healthy:**
- Mobile App proceeding well, ahead of schedule"
```

---

## Task Management

### Find and Complete Stale Tasks

```
User: "What tasks have been in progress for too long?"

AI workflow:
1. GetTasks → status=in_progress
2. Filter by started date > 7 days ago

AI response:
"Found 3 tasks in progress for over a week:

1. **Database optimization** (12 days)
   - 8h logged of 6h estimate
   - Assignee: Thomas
   → Might be blocked or underestimated

2. **Email templates** (9 days)
   - 2h logged of 8h estimate
   - Assignee: None
   → Needs assignment

3. **API documentation** (8 days)
   - 10h logged of 4h estimate
   - Assignee: Developer
   → Scope creep, update estimate

Want me to update any of these?"
```

---

## Tips for Effective Workflows

1. **Start sessions with GetUserContext** - establishes context
2. **Use bulk operations** - more efficient than individual calls
3. **Leverage AI tools** - health checks, estimates, decomposition
4. **Combine tools** - chain calls for comprehensive answers
5. **Be specific** - include project/task names for faster resolution
