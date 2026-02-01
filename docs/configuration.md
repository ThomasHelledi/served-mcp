# Configuration

Complete configuration reference for Served MCP Server.

---

## Environment Variables

### Required

| Variable | Description | Example |
|----------|-------------|---------|
| `SERVED_TOKEN` | Your API token | `sk_live_xxx...` |
| `SERVED_TENANT` | Workspace slug | `my-company` |

### Optional

| Variable | Description | Default |
|----------|-------------|---------|
| `SERVED_API_URL` | API base URL | `https://apis.unifiedhq.ai` |
| `SERVED_MCP_TRACING` | Enable tracing | `false` |
| `SERVED_MCP_LOG_LEVEL` | Log verbosity | `Information` |
| `FORGE_API_KEY` | Forge analytics key | - |

---

## Claude Desktop Config

### Minimal Configuration

```json
{
  "mcpServers": {
    "served": {
      "command": "served-mcp",
      "env": {
        "SERVED_TOKEN": "your-token",
        "SERVED_TENANT": "your-workspace"
      }
    }
  }
}
```

### Full Configuration

```json
{
  "mcpServers": {
    "served": {
      "command": "served-mcp",
      "env": {
        "SERVED_API_URL": "https://apis.unifiedhq.ai",
        "SERVED_TOKEN": "your-token",
        "SERVED_TENANT": "your-workspace",
        "SERVED_MCP_TRACING": "true",
        "SERVED_MCP_LOG_LEVEL": "Debug"
      }
    }
  }
}
```

### Running from Source

```json
{
  "mcpServers": {
    "served": {
      "command": "dotnet",
      "args": ["run", "--project", "/path/to/Served.MCP"],
      "env": {
        "SERVED_TOKEN": "your-token",
        "SERVED_TENANT": "your-workspace"
      }
    }
  }
}
```

### Docker Configuration

```json
{
  "mcpServers": {
    "served": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "SERVED_TOKEN=your-token",
        "-e", "SERVED_TENANT=your-workspace",
        "ghcr.io/unifiedhq/served-mcp:latest"
      ]
    }
  }
}
```

---

## Observability

### Enable Tracing

```bash
export SERVED_MCP_TRACING=true
export FORGE_API_KEY="your-forge-key"
```

When enabled, every tool call is traced with:

| Attribute | Description |
|-----------|-------------|
| `mcp.tool.name` | Tool name (e.g., `GetProjects`) |
| `mcp.tool.success` | Success/failure |
| `mcp.tool.duration_ms` | Execution time |
| `mcp.session.id` | Session identifier |
| `served.tenant` | Workspace slug |

### View Analytics

- **Forge Dashboard:** [forge.unifiedhq.ai/analytics](https://forge.unifiedhq.ai/analytics)
- **Workspace Analytics:** [unifiedhq.ai/app/analytics](https://unifiedhq.ai/app/analytics)

---

## Log Levels

Set `SERVED_MCP_LOG_LEVEL` to control verbosity:

| Level | Description |
|-------|-------------|
| `Trace` | Everything (very verbose) |
| `Debug` | Debugging information |
| `Information` | Normal operation (default) |
| `Warning` | Potential issues |
| `Error` | Errors only |

---

## API Environments

| Environment | URL | Use Case |
|-------------|-----|----------|
| **Production** | `https://apis.unifiedhq.ai` | Default, recommended |
| **Local Dev** | `http://localhost:5010` | Development/testing |

---

## Security Best Practices

1. **Never commit tokens** to source control
2. **Use environment variables** instead of hardcoding
3. **Rotate tokens** periodically
4. **Use minimum scopes** needed for your use case
5. **Monitor usage** via Forge analytics

---

## Multiple Workspaces

To switch workspaces, update `SERVED_TENANT` and restart the MCP server.

For multi-workspace setups, you can configure multiple MCP servers:

```json
{
  "mcpServers": {
    "served-workspace1": {
      "command": "served-mcp",
      "env": {
        "SERVED_TOKEN": "token1",
        "SERVED_TENANT": "workspace1"
      }
    },
    "served-workspace2": {
      "command": "served-mcp",
      "env": {
        "SERVED_TOKEN": "token2",
        "SERVED_TENANT": "workspace2"
      }
    }
  }
}
```
