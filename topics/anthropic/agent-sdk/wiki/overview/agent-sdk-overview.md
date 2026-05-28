# Agent SDK Overview

The Claude Agent SDK is a Python and TypeScript library that gives you the same agentic capabilities that power Claude Code — built-in tool execution, session management, hooks, and MCP connectivity — as a programmable library you can embed in your own applications.

## What It Is

The Agent SDK wraps Claude's agentic loop so you don't have to implement it yourself. When you call `query()`, Claude autonomously decides which tools to call, executes them, observes the results, and continues until the task is done. You get a stream of messages describing what happened.

This contrasts with the Anthropic Client SDK, where you send a single message, inspect the response for tool calls, execute them yourself, and loop manually. The Agent SDK handles all of that.

## Installation and Authentication

```bash
npm install @anthropic-ai/claude-agent-sdk   # TypeScript
pip install claude-agent-sdk                  # Python
```

Authentication uses `ANTHROPIC_API_KEY` by default, with environment variable flags for Bedrock (`CLAUDE_CODE_USE_BEDROCK=1`), Vertex AI (`CLAUDE_CODE_USE_VERTEX=1`), Azure (`CLAUDE_CODE_USE_FOUNDRY=1`), and Claude Platform on AWS (`CLAUDE_CODE_USE_ANTHROPIC_AWS=1`).

## The `query()` Function

`query()` is the main entry point. It accepts a prompt and options, and returns an async iterator that yields messages as the agent works.

```python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async def main():
    async for message in query(
        prompt="Find all TODO comments and summarize them",
        options=ClaudeAgentOptions(allowed_tools=["Read", "Glob", "Grep"]),
    ):
        if isinstance(message, ResultMessage) and message.subtype == "success":
            print(message.result)

asyncio.run(main())
```

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "Find all TODO comments and summarize them",
  options: { allowedTools: ["Read", "Glob", "Grep"] }
})) {
  if (message.type === "result" && message.subtype === "success") {
    console.log(message.result);
  }
}
```

## Built-in Tools

The Agent SDK ships with tools for all common development tasks:

| Tool | Purpose |
| ---- | ------- |
| Read, Write, Edit | File I/O and editing |
| Bash | Terminal commands, git, scripts |
| Monitor | Watch background scripts, react to output |
| Glob, Grep | File search and content search |
| WebSearch, WebFetch | Web access |
| AskUserQuestion | Interactive clarifying questions |

Pass the names you want to allow in `allowedTools`. Tools not listed are available but require a permission prompt (or `canUseTool` callback in `default` mode, or are blocked in `dontAsk` mode).

## Capabilities

The SDK exposes the full Claude Code feature surface:

- **[[mcp]]**: Connect to external services (GitHub, Slack, databases) via the Model Context Protocol
- **[[custom-tools/defining-tools|Custom tools]]**: Wrap your own functions in in-process MCP servers
- **[[hooks/hooks|Hooks]]**: Run code before/after tool calls, block dangerous operations, log, redirect
- **[[subagents/subagents|Subagents]]**: Spawn specialized sub-agents for parallel or isolated subtasks
- **[[sessions/sessions|Sessions]]**: Persist and resume conversation history across calls
- **[[permissions/permissions|Permissions]]**: Fine-grained control over what Claude can do

## Comparison

| | Agent SDK | Managed Agents | Client SDK |
|---|---|---|---|
| Runs in | Your process | Anthropic infra | Your process |
| Tool loop | Automatic | Hosted | You implement |
| Custom tools | In-process functions | You execute + return | You implement |
| Session state | Local JSONL files | Anthropic-hosted | You implement |
| Best for | Local/embedded agents | Production async agents | Full API control |

## Note on Subscription Plans (June 2026)

Starting June 15, 2026, Agent SDK usage on subscription plans draws from a separate monthly Agent SDK credit, distinct from interactive Claude usage limits.

## Related

- [[custom-tools/defining-tools]]
- [[mcp/mcp]]
- [[permissions/permissions]]
- [[hooks/hooks]]
- [[sessions/sessions]]
- [[subagents/subagents]]

## Sources

- [[../../raw/overview/index|Agent SDK Overview]] — Anthropic, 2025.
- [[../../raw/quickstart/index|Agent SDK Quickstart]] — Anthropic, 2025.
