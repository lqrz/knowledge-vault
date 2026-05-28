# Connect to External Tools with MCP

> Source: https://code.claude.com/docs/en/agent-sdk/mcp

The Model Context Protocol (MCP) is an open standard for connecting AI agents to external tools and data sources. With MCP, your agent can query databases, integrate with APIs like Slack and GitHub, and connect to other services.

## Quickstart

```typescript
for await (const message of query({
  prompt: "Use the docs MCP server to explain what hooks are in Claude Code",
  options: {
    mcpServers: {
      "claude-code-docs": { type: "http", url: "https://code.claude.com/docs/mcp" }
    },
    allowedTools: ["mcp__claude-code-docs__*"]
  }
}))
```

## Adding MCP Servers

### In code

```typescript
options: {
  mcpServers: {
    filesystem: {
      command: "npx",
      args: ["-y", "@modelcontextprotocol/server-filesystem", "/Users/me/projects"]
    }
  },
  allowedTools: ["mcp__filesystem__*"]
}
```

### From `.mcp.json`

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/Users/me/projects"]
    }
  }
}
```

Loaded when `project` setting source is enabled (default).

## Tool Naming Convention

`mcp__<server-name>__<tool-name>` — e.g., `mcp__github__list_issues`. Wildcard: `mcp__github__*`.

## Transport Types

**stdio**: local processes via stdin/stdout. Use for servers run on the same machine (`command` + `args` + optional `env`).

**HTTP/SSE**: cloud-hosted servers.
- `"type": "sse"` — SSE transport
- `"type": "http"` — streamable HTTP (also accepts `"streamable-http"` in JSON config)
- Supports `headers` for auth

**SDK MCP servers**: custom tools defined directly in your application code. See custom-tools guide.

## Authentication

Pass credentials via `env` field (stdio) or `headers` field (HTTP/SSE):

```python
mcp_servers={
    "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {"GITHUB_TOKEN": os.environ["GITHUB_TOKEN"]},
    }
}
```

For OAuth2: complete the flow in your app, then pass the access token via headers.

## Tool Search

When many MCP tools are configured, tool search withholds definitions from context and loads only relevant ones per turn. Enabled by default. See tool-search guide.

## Error Handling

Check the `system` init message for connection status:

```typescript
if (message.type === "system" && message.subtype === "init") {
  const failedServers = message.mcp_servers.filter((s) => s.status !== "connected");
  if (failedServers.length > 0) console.warn("Failed to connect:", failedServers);
}
```

## Prefer `allowedTools` over permission modes for MCP

`acceptEdits` does not auto-approve MCP tools. `bypassPermissions` does but is overly broad. Use a wildcard in `allowedTools` instead.
