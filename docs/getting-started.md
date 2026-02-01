# Getting Started with Served MCP

This guide will help you install and configure the Served MCP Server for use with AI assistants.

---

## Prerequisites

- .NET 10.0 or later
- A Served/UnifiedHQ account
- An API key ([get one here](https://unifiedhq.ai/app/settings/api-keys))
- Claude Desktop, Cursor, or another MCP-compatible client

---

## Installation

### Option 1: NuGet Global Tool (Recommended)

```bash
dotnet tool install -g Served.MCP
```

After installation, the `served-mcp` command is available globally.

### Option 2: Run from NuGet Package

```bash
dotnet tool run served-mcp
```

### Option 3: Docker

```bash
docker pull ghcr.io/unifiedhq/served-mcp:latest
```

---

## Configuration

### Claude Desktop

Add to your Claude Desktop config file:

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

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

### Cursor IDE

Add to Cursor's MCP configuration:

```json
{
  "mcpServers": {
    "served": {
      "command": "served-mcp",
      "env": {
        "SERVED_TOKEN": "your-api-token",
        "SERVED_TENANT": "your-workspace"
      }
    }
  }
}
```

---

## Get Your API Token

1. Log in to [unifiedhq.ai](https://unifiedhq.ai)
2. Go to **Settings** > **API Keys**
3. Click **Create API Key**
4. Select scopes: `projects`, `tasks`, `timetracking`, `customers`
5. Copy the token

---

## First Steps

Once configured, restart your AI client and try these commands:

### 1. Get Your Context (Always First)

```
"What workspaces do I have access to?"
```

The AI will call `GetUserContext` to retrieve your profile and available workspaces.

### 2. List Projects

```
"Show me my active projects"
```

### 3. Create a Task

```
"Create a task called 'Review PR #123' in the Website project"
```

### 4. Log Time

```
"Log 2 hours on the code review task"
```

---

## Verify Installation

Test that the MCP server is working:

```bash
# Run directly to check for errors
served-mcp

# Should output MCP protocol initialization
# Press Ctrl+C to exit
```

If you see JSON output starting with `{"jsonrpc":"2.0"`, the server is working.

---

## Troubleshooting

### "Command not found: served-mcp"

The global tool isn't in your PATH. Try:

```bash
# Add .NET tools to PATH
export PATH="$PATH:$HOME/.dotnet/tools"

# Or run with full path
~/.dotnet/tools/served-mcp
```

### "Unauthorized" errors

- Check that `SERVED_TOKEN` is set correctly
- Verify the token hasn't expired
- Ensure the token has required scopes

### "Tenant not found"

- Check that `SERVED_TENANT` matches your workspace slug exactly
- The slug is the URL-friendly name (lowercase, no spaces)

### Claude doesn't see the tools

1. Restart Claude Desktop completely
2. Check the config file path is correct
3. Verify JSON syntax is valid

---

## Next Steps

- [Configuration Guide](./configuration.md) - All environment variables
- [Tools Reference](./tools/) - Complete tool documentation
- [Examples](./examples/) - Common workflows
