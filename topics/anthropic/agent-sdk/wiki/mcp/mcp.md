# Connecting MCP Servers

The Model Context Protocol (MCP) is an open standard for connecting Claude to external tools and data sources. The Agent SDK supports three transport types — stdio (local processes), HTTP/SSE (remote servers), and in-process SDK servers — and reads credentials from environment variables or request headers.

## Transport Types

**stdio** — local processes that communicate via stdin/stdout. Use when you run the MCP server on the same machine as the SDK.

```python
mcp_servers={
    "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {"GITHUB_TOKEN": os.environ["GITHUB_TOKEN"]},
    }
}
```

**HTTP / SSE** — cloud-hosted or remote servers identified by URL. Pass `"type": "http"` or `"type": "sse"`. Headers carry authentication.

```python
mcp_servers={
    "remote-api": {
        "type": "http",
        "url": "https://api.example.com/mcp",
        "headers": {"Authorization": f"Bearer {os.environ['API_TOKEN']}"},
    }
}
```

**SDK MCP servers** — custom tools defined in your application code. No separate process. See [[../custom-tools/defining-tools]].

## Tool Name Convention

All MCP tools follow: `mcp__{server_name}__{tool_name}`

The `server_name` is the key in `mcpServers`. Wildcards work: `mcp__github__*` covers all tools on the `github` server.

## Allowing MCP Tools

MCP tools require explicit permission. Use `allowedTools` with a wildcard rather than `bypassPermissions`:

```python
options = ClaudeAgentOptions(
    mcp_servers={"github": {...}},
    allowed_tools=["mcp__github__list_issues", "mcp__github__search_issues"],
    # or: allowed_tools=["mcp__github__*"]
)
```

`acceptEdits` does not auto-approve MCP tools. `bypassPermissions` does, but it also disables all other safety prompts — too broad for most uses.

## From `.mcp.json`

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "${GITHUB_TOKEN}" }
    }
  }
}
```

Loaded when the `project` setting source is enabled (default). The `${VARIABLE}` syntax expands environment variables at runtime.

## Discovering Available Tools

Check the `system` init message to see what the server exposes:

```typescript
if (message.type === "system" && message.subtype === "init") {
  console.log("MCP servers:", message.mcp_servers);
}
```

## Error Handling

Init message includes connection status for each server:

```python
if isinstance(message, SystemMessage) and message.subtype == "init":
    failed = [s for s in message.data.get("mcp_servers", []) if s.get("status") != "connected"]
    if failed:
        print(f"Failed to connect: {failed}")
```

Common causes: missing env vars, server not installed, invalid connection string, network issues.

## Tool Search for Large MCP Catalogs

When many tools are configured, tool definitions can consume large portions of the context window. Tool search (enabled by default) withholds definitions and loads only what's needed per turn. See [[../tool-search/tool-search]].

## Common Servers

- `@modelcontextprotocol/server-github` — GitHub issues, PRs, repos
- `@modelcontextprotocol/server-filesystem` — filesystem access
- `@modelcontextprotocol/server-postgres` — PostgreSQL queries
- `@playwright/mcp` — browser automation

## Related

- [[../custom-tools/defining-tools]]
- [[../tool-search/tool-search]]
- [[../permissions/permissions]]

## Sources

- [[../../raw/mcp/index|Agent SDK: Connect to External Tools with MCP]] — Anthropic, 2025.
