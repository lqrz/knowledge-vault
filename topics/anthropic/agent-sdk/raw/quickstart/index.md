# Agent SDK Quickstart

> Source: https://code.claude.com/docs/en/agent-sdk/quickstart

## Setup

1. Create project folder: `mkdir my-agent && cd my-agent`
2. Install SDK: `npm install @anthropic-ai/claude-agent-sdk` or `pip install claude-agent-sdk`
3. Set API key: `ANTHROPIC_API_KEY=your-api-key` in `.env`

## Build a Bug-Fixing Agent

Create `utils.py` with intentional bugs (division-by-zero, TypeError), then:

```python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions, AssistantMessage, ResultMessage

async def main():
    async for message in query(
        prompt="Review utils.py for bugs that would cause crashes. Fix any issues you find.",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Edit", "Glob"],
            permission_mode="acceptEdits",
        ),
    ):
        if isinstance(message, AssistantMessage):
            for block in message.content:
                if hasattr(block, "text"):
                    print(block.text)
                elif hasattr(block, "name"):
                    print(f"Tool: {block.name}")
        elif isinstance(message, ResultMessage):
            print(f"Done: {message.subtype}")

asyncio.run(main())
```

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "Review utils.py for bugs that would cause crashes. Fix any issues you find.",
  options: {
    allowedTools: ["Read", "Edit", "Glob"],
    permissionMode: "acceptEdits"
  }
})) {
  if (message.type === "assistant" && message.message?.content) {
    for (const block of message.message.content) {
      if ("text" in block) console.log(block.text);
      else if ("name" in block) console.log(`Tool: ${block.name}`);
    }
  } else if (message.type === "result") {
    console.log(`Done: ${message.subtype}`);
  }
}
```

## Key Concepts

**`query()`**: main entry point, returns an async iterator. `async for` streams messages as Claude works.

**`prompt`**: what you want Claude to do.

**`options`**: configuration. Includes `allowedTools`, `permissionMode`, `systemPrompt`, `mcpServers`, etc.

The loop runs as Claude thinks, calls tools, observes results, and decides next steps. Ends when Claude finishes or hits an error.

## Tool Combinations

| Tools | What the agent can do |
|-------|-----------------------|
| `Read`, `Glob`, `Grep` | Read-only analysis |
| `Read`, `Edit`, `Glob` | Analyze and modify code |
| `Read`, `Edit`, `Bash`, `Glob`, `Grep` | Full automation |

## Permission Modes

| Mode | Behavior | Use case |
|------|----------|----------|
| `acceptEdits` | Auto-approves file edits and common filesystem commands | Trusted development |
| `dontAsk` | Denies anything not in `allowedTools` | Locked-down headless agents |
| `auto` (TS only) | Model classifier approves/denies each call | Autonomous agents with guardrails |
| `bypassPermissions` | Runs every tool without prompts | Sandboxed CI |
| `default` | Requires `canUseTool` callback | Custom approval flows |
