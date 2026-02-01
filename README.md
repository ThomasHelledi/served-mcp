# Served MCP Server

MCP (Model Context Protocol) Server for AI assistants to interact with the [Served](https://served.dk) and [UnifiedHQ](https://unifiedhq.ai) platforms.

[![NuGet](https://img.shields.io/nuget/v/Served.MCP.svg)](https://www.nuget.org/packages/Served.MCP)
[![.NET](https://img.shields.io/badge/.NET-10.0-512BD4)](https://dotnet.microsoft.com/)
[![MCP](https://img.shields.io/badge/MCP-Compatible-blue)](https://modelcontextprotocol.io)
[![License: NON-AI-MIT](https://img.shields.io/badge/License-NON--AI--MIT-red.svg)](./LICENSE)

---

## What is MCP?

[Model Context Protocol](https://modelcontextprotocol.io) (MCP) is an open protocol that enables AI assistants like Claude to securely access external tools and data sources. This server provides 40+ tools for managing your Served workspace.

---

## Quick Start

### 1. Install via NuGet

```bash
dotnet tool install -g Served.MCP
```

### 2. Configure Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "served": {
      "command": "served-mcp",
      "env": {
        "SERVED_API_URL": "https://apis.unifiedhq.ai",
        "SERVED_TOKEN": "your-api-token",
        "SERVED_TENANT": "your-workspace-slug"
      }
    }
  }
}
```

### 3. Get Your API Token

1. Go to [unifiedhq.ai/app/settings/api-keys](https://unifiedhq.ai/app/settings/api-keys)
2. Create a new API key with required scopes
3. Copy the token to your config

### 4. Start Using

In Claude, try:
- "Show me my projects"
- "Create a task for the Website project"
- "Log 2 hours on task #123"
- "What's the health of my projects?"

---

## Documentation

| Guide | Description |
|-------|-------------|
| [Getting Started](./docs/getting-started.md) | Installation and setup |
| [Configuration](./docs/configuration.md) | Environment variables and options |
| [Tools Reference](./docs/tools/) | Complete tool documentation |
| [Examples](./docs/examples/) | Common use cases |

---

## Available Tools

### Context Tools

| Tool | Description |
|------|-------------|
| `GetUserContext` | Get user profile and workspaces. **Call this first.** |
| `GetTenantContext` | Get detailed tenant info (settings, categories) |
| `GetProjectContext` | Get project with tasks, team, and activity |

### Project Management

| Tool | Description |
|------|-------------|
| `GetProjects` | List all projects |
| `GetProjectDetails` | Get detailed project info |
| `CreateProject` | Create new project |
| `UpdateProject` | Update project |
| `DeleteProject` | Delete project |

### Task Management

| Tool | Description |
|------|-------------|
| `GetTasks` | Get tasks for project |
| `GetTaskDetails` | Get task details |
| `CreateTask` | Create new task |
| `UpdateTask` | Update task |
| `DeleteTask` | Delete task |
| `CreateTasksBulk` | Bulk create tasks |

### Time Tracking

| Tool | Description |
|------|-------------|
| `CreateTimeRegistration` | Log time entry |
| `GetTimeRegistrations` | Get time entries |
| `SuggestTimeEntries` | AI suggestions for logging |
| `AnalyzeTimePatterns` | Analyze time patterns |

### AI Intelligence

| Tool | Description |
|------|-------------|
| `AnalyzeProjectHealth` | Health check with recommendations |
| `SuggestTaskDecomposition` | Break down complex tasks |
| `EstimateEffort` | AI effort estimation |
| `FindSimilarProjects` | Find similar projects |

### DevOps

| Tool | Description |
|------|-------------|
| `GetDevOpsRepositories` | List Git repos |
| `GetPullRequests` | Get pull requests |
| `GetPipelineRuns` | Get CI/CD runs |
| `GetJobLog` | Get job output |

See [Tools Reference](./docs/tools/) for complete documentation.

---

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `SERVED_API_URL` | API base URL | `https://apis.unifiedhq.ai` |
| `SERVED_TOKEN` | Your API token | Required |
| `SERVED_TENANT` | Workspace slug | Required |
| `SERVED_MCP_TRACING` | Enable tracing | `false` |

### Enable Observability

```bash
export SERVED_MCP_TRACING=true
```

View analytics at [forge.unifiedhq.ai/analytics](https://forge.unifiedhq.ai/analytics)

---

## Alternative Installation

### Run from Source

```bash
git clone https://github.com/UnifiedHQ/served-mcp.git
cd served-mcp
dotnet run
```

### Docker

```bash
docker run -e SERVED_TOKEN=xxx -e SERVED_TENANT=yyy ghcr.io/unifiedhq/served-mcp:latest
```

---

## Support

| Resource | Link |
|----------|------|
| **Documentation** | [docs.served.dk/mcp](https://docs.served.dk/mcp) |
| **MCP Protocol** | [modelcontextprotocol.io](https://modelcontextprotocol.io) |
| **Issues** | [GitHub Issues](https://github.com/UnifiedHQ/served-mcp/issues) |
| **Discord** | [UnifiedHQ Community](https://discord.gg/unifiedhq) |

---

## License

This documentation is licensed under [NON-AI-MIT](./LICENSE).

The MCP server is available via [NuGet](https://www.nuget.org/packages/Served.MCP).

> **Notice:** This repository contains documentation only. The MCP server source code is proprietary.
> AI training on this content is prohibited under the license terms.

---

Built with care by [UnifiedHQ](https://unifiedhq.ai)
