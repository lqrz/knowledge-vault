# Tool Access Control

The Agent SDK lets you control which tools Claude can see and which it can call without a permission prompt. Two independent layers govern this: **availability** and **permission**.

## Tool Name Format

MCP tools follow a fixed naming convention:

```
mcp__{server_name}__{tool_name}
```

The `server_name` is the key you pass to `mcp_servers` / `mcpServers` in `query()`. So `get_temperature` in server `weather` becomes `mcp__weather__get_temperature`. Wildcards are supported: `mcp__weather__*` matches every tool on the `weather` server.

## Availability vs. Permission

| Layer        | Controlled by                            | Effect                                                                |
| :----------- | :--------------------------------------- | :-------------------------------------------------------------------- |
| Availability | `tools` option, bare `disallowedTools`   | Whether the tool appears in Claude's context at all                   |
| Permission   | `allowedTools`, scoped `disallowedTools` | Whether a call is auto-approved or routed through the permission flow |

Keeping a tool out of context is stronger than denying permission. If the tool isn't in context, Claude never attempts to call it and no turn is wasted.

## The `tools` Option

`tools` filters the built-in tools that appear in Claude's context. MCP tools are unaffected.

```python
# Only Read and Grep built-ins are visible; all MCP tools remain available
options = ClaudeAgentOptions(tools=["Read", "Grep"])

# Remove all built-ins; Claude can only use custom MCP tools
options = ClaudeAgentOptions(tools=[])
```

## `allowedTools`

Tools listed in `allowedTools` / `allowed_tools` are pre-approved and run without a permission prompt. Unlisted tools are still available but go through the normal permission flow.

```python
options = ClaudeAgentOptions(
    mcp_servers={"weather": weather_server},
    allowed_tools=["mcp__weather__get_temperature"],
)
```

```typescript
options: {
  mcpServers: { weather: weatherServer },
  allowedTools: ["mcp__weather__get_temperature", "Read", "Grep"]
}
```

## `disallowedTools`

Two forms with different effects:

**Bare name** (`"Bash"`) — removes the tool from context entirely, same as omitting it from `tools`. Claude never sees it.

**Scoped rule** (`"Bash(rm *)"`) — leaves the tool in context but blocks calls matching the pattern. Claude may still attempt the call and waste a turn on the denial.

Prefer bare-name disallowing when you want Claude to never attempt a tool. Use scoped rules only when you need fine-grained blocking within a tool (e.g., allow most Bash commands but not destructive ones).

## Common Patterns

**Read-only agent** — expose inspection tools, block all write tools:
```python
ClaudeAgentOptions(allowed_tools=["Read", "Glob", "Grep"])
# Bash, Edit, Write are not listed in tools= so they're absent from context
```

**MCP-only agent** — strip all built-ins, expose only your custom server:
```python
ClaudeAgentOptions(
    tools=[],
    mcp_servers={"myserver": my_server},
    allowed_tools=["mcp__myserver__*"],
)
```

**Block a specific built-in operation**:
```python
ClaudeAgentOptions(disallowed_tools=["Bash(rm *)"])
```

## Related

- [[defining-tools]]
- [[error-handling]]

## Sources

- [[../../raw/custom-tools/index|Agent SDK: Give Claude Custom Tools]] — Anthropic, 2025.
