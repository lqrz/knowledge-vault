# Agent Loop & Tool Detection

The Agent SDK runs an autonomous loop inside `query()`. Understanding the message types it emits — and how the loop decides to execute a tool — is key to building correct handlers.

## The Loop at a Glance

![[agent-loop-tool-detection.svg]]

## How Tool Detection Works

Every turn, Claude responds with an **AssistantMessage** whose `content` array can hold two kinds of blocks:

- **TextBlock** (`type: "text"`) — narration or a final answer
- **ToolUseBlock** (`type: "tool_use"`) — a tool call intent

The SDK inspects the content array after each AssistantMessage. If **any block has `type === "tool_use"`**, the loop continues: it executes each tool and injects a **UserMessage** carrying the results back as `ToolResultBlock`s (`type: "tool_result"`), matched to the original call by `tool_use_id`. Claude then gets another turn.

When an AssistantMessage contains **no** `tool_use` blocks, the loop ends and a **ResultMessage** is emitted.

## Message Types

### SystemMessage — emitted once at session start

```json
{
  "type": "system",
  "subtype": "init",
  "session_id": "sess_abc123..."
}
```

`subtype` is always `"init"`. Use `session_id` to resume sessions later.

### AssistantMessage — Claude's turn

```json
{
  "type": "assistant",
  "message": {
    "role": "assistant",
    "content": [
      { "type": "text",     "text": "I'll read the file first." },
      { "type": "tool_use", "id": "toolu_01XyZ...", "name": "Read",
        "input": { "file_path": "/src/main.py" } }
    ]
  }
}
```

One message may contain multiple ToolUseBlocks — the SDK executes all of them and collects all results before sending back a single UserMessage.

### UserMessage — SDK-injected tool results

```json
{
  "type": "user",
  "message": {
    "role": "user",
    "content": [
      { "type": "tool_result",
        "tool_use_id": "toolu_01XyZ...",
        "content": [ { "type": "text", "text": "def main(): ..." } ] }
    ]
  }
}
```

`tool_use_id` matches the `id` field from the corresponding ToolUseBlock. You do not construct this message — the SDK does.

### ResultMessage — loop ends here

```json
{
  "type": "result",
  "subtype": "success",
  "result": "Found 3 bugs and fixed them.",
  "cost_usd": 0.0042
}
```

`subtype` is one of `"success"`, `"error"`, or `"interrupted"`.

## Detecting Tool Calls in Your Handler

**Python:**
```python
from claude_agent_sdk import AssistantMessage, ResultMessage
from anthropic.types import ToolUseBlock, TextBlock

async for message in query(prompt, options):
    if isinstance(message, AssistantMessage):
        for block in message.content:
            if isinstance(block, ToolUseBlock):
                print(f"Tool: {block.name}, input: {block.input}")
            elif isinstance(block, TextBlock):
                print(block.text)
    elif isinstance(message, ResultMessage):
        print(f"Done ({message.subtype}): {message.result}")
```

**TypeScript:**
```typescript
for await (const message of query({ prompt, options })) {
  if (message.type === "assistant") {
    for (const block of message.message.content) {
      if (block.type === "tool_use") {
        console.log(`Tool: ${block.name}`, block.input);
      } else if (block.type === "text") {
        console.log(block.text);
      }
    }
  } else if (message.type === "result") {
    console.log(`Done (${message.subtype}):`, message.result);
  }
}
```

## Subagent Detection

When Claude invokes a subagent, it emits a ToolUseBlock where `name === "Agent"` (or `"Task"` in older SDK versions). Messages originating from inside a subagent carry `parent_tool_use_id`.

## Related

- [[agent-sdk-overview]]
- [[../hooks/hooks]]
- [[../permissions/permissions]]
- [[../subagents/subagents]]
- [[../custom-tools/defining-tools]]

## Sources

- [[../../raw/overview/index|Agent SDK Overview]] — Anthropic, 2025.
- [[../../raw/quickstart/index|Agent SDK Quickstart]] — Anthropic, 2025.
- [[../../raw/subagents/index|Subagents]] — Anthropic, 2025.
