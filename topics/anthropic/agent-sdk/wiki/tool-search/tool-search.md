# Tool Search

Tool search lets agents work with large tool catalogs without paying the context window cost of loading all tool definitions upfront. Instead of loading every definition on every turn, the agent searches for relevant tools as needed and loads the 3-5 best matches.

## The Problem It Solves

Tool definitions are expensive: 50 tools can consume 10-20K tokens per turn. Tool selection accuracy also degrades when more than 30-50 tools are loaded simultaneously. Tool search addresses both issues by keeping definitions out of context until they're needed.

## How It Works

When active, tool definitions are withheld from context. The agent receives a summary of what tools exist and performs a search step when it needs a capability not yet loaded. The 3-5 most relevant tools are pulled into context, where they remain for subsequent turns. If context compaction clears earlier turns, the agent may search again.

The search step adds one round-trip the first time Claude uses a tool. For large catalogs this is worth it; for fewer than ~10 tools, loading everything upfront is typically faster.

**Model support:** Sonnet 4+ and Opus 4+. Haiku models do not support tool search.

## Configuration

Control via the `ENABLE_TOOL_SEARCH` environment variable set in the `env` option:

```python
options = ClaudeAgentOptions(
    mcp_servers={"enterprise-tools": {"type": "http", "url": "https://tools.example.com/mcp"}},
    allowed_tools=["mcp__enterprise-tools__*"],
    env={"ENABLE_TOOL_SEARCH": "auto:5"},
)
```

| Value | Behavior |
|-------|----------|
| (unset) or `"true"` | Always on — tool definitions never loaded into context |
| `"auto"` | Activates when definitions exceed 10% of context window |
| `"auto:N"` | Activates when definitions exceed N% of context window |
| `"false"` | Off — all tool definitions loaded on every turn |

`auto` mode lets small tool sets load normally while large ones activate search. This is a good default for applications where tool count varies.

## Optimizing Discovery

Search matches queries against tool names and descriptions. To improve which tools get found:

**Specific names**: `search_slack_messages` surfaces for more query variations than `query_slack`.

**Keyword-rich descriptions**: "Search Slack messages by keyword, channel, or date range" matches more queries than "Query Slack".

**System prompt context**: tell the agent what categories of tools are available so it knows to search for them:
```
You can search for tools to interact with Slack, GitHub, and Jira.
```

## Limits

- Max tools in catalog: 10,000
- Search results per query: 3-5 tools

## Related

- [[../mcp/mcp]] — tool search applies to both remote MCP servers and custom SDK MCP servers
- [[../custom-tools/defining-tools]] — every tool in a server's array consumes context; tool search defers this cost

## Sources

- [[../../raw/tool-search/index|Agent SDK: Scale to Many Tools with Tool Search]] — Anthropic, 2025.
