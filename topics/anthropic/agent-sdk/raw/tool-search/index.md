# Scale to Many Tools with Tool Search

> Source: https://code.claude.com/docs/en/agent-sdk/tool-search

Tool search enables agents to work with hundreds or thousands of tools by dynamically discovering and loading them on demand, rather than loading all tool definitions upfront.

## Why It Matters

- 50 tools can use 10-20K context window tokens
- Tool selection accuracy degrades beyond 30-50 tools loaded at once

## How It Works

When tool search is active, tool definitions are withheld from context. The agent receives a summary of available tools, searches for relevant ones when needed, and loads 3-5 most relevant tools per search. Previously discovered tools stay in context until compaction.

The first use of a tool adds one extra round-trip (the search step). For large tool sets, this is offset by smaller context on every turn. With fewer than ~10 tools, loading everything upfront is typically faster.

**Model support:** Claude Sonnet 4 or later, Claude Opus 4 or later. No Haiku support.

**Max catalog size:** 10,000 tools.

## Configuration (`ENABLE_TOOL_SEARCH`)

| Value | Behavior |
|-------|----------|
| (unset) or `true` | Always on. Tool definitions never loaded into context. |
| `auto` | Activates when combined tool definitions exceed 10% of context window |
| `auto:N` | Same as `auto` with custom percentage threshold |
| `false` | Off. All tools loaded into context on every turn. |

Set via the `env` option on `query()`:

```typescript
options: {
  mcpServers: { "enterprise-tools": { type: "http", url: "..." } },
  allowedTools: ["mcp__enterprise-tools__*"],
  env: { ENABLE_TOOL_SEARCH: "auto:5" }
}
```

## Optimizing Tool Discovery

Search mechanism matches queries against tool names and descriptions:
- `search_slack_messages` > `query_slack` (more specific)
- "Search Slack messages by keyword, channel, or date range" > "Query Slack" (more keywords)

Add system prompt context listing available tool categories:
```
You can search for tools to interact with Slack, GitHub, and Jira.
```
