# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in Served.MCP, please report it responsibly:

1. **Do NOT** create a public GitHub issue
2. Email security@served.dk with details
3. Include steps to reproduce if possible
4. We will respond within 48 hours

## Supported Versions

| Version | Supported |
|---------|-----------|
| 2026.x  | ✅ Yes    |
| 2025.x  | ⚠️ Security fixes only |
| < 2025  | ❌ No     |

## Security Best Practices

When using Served.MCP:

1. **Never commit API tokens** to config files
2. **Use environment variables** for `SERVED_TOKEN`
3. **Rotate tokens** periodically
4. **Use minimum required scopes**
5. **Review tool calls** made by AI assistants

## MCP-Specific Security

- MCP servers run locally with your credentials
- All API calls are made on your behalf
- Review the tools available before granting access
- Consider using read-only API keys for exploration

## License Notice

This project is licensed under NON-AI-MIT. AI/ML training on this content is prohibited.
