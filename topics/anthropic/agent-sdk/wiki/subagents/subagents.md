# Subagents

Subagents are separate agent instances your main agent can spawn to handle focused subtasks. They run in their own fresh conversation context — intermediate tool calls and results stay inside the subagent. Only the subagent's final message returns to the parent.

## Why Use Subagents

**Context isolation**: a subagent exploring dozens of files doesn't accumulate that content in the main conversation. The parent receives a concise summary.

**Parallelization**: multiple subagents can run concurrently. A code review that runs style, security, and test-coverage subagents simultaneously can be dramatically faster than running them sequentially.

**Specialized instructions**: each subagent has its own system prompt, tailored for its specific task. Detailed knowledge about SQL migration strategies or OWASP patterns doesn't pollute the main agent's context.

**Tool restrictions**: subagents can be limited to specific tools, reducing the risk of unintended side effects.

## Defining Subagents

Use the `agents` parameter in `query()` options. Include `Agent` in `allowedTools` so subagent invocations are auto-approved.

```python
from claude_agent_sdk import AgentDefinition

async for message in query(
    prompt="Review the authentication module for security issues",
    options=ClaudeAgentOptions(
        allowed_tools=["Read", "Grep", "Glob", "Agent"],
        agents={
            "code-reviewer": AgentDefinition(
                description="Expert code review specialist. Use for quality, security, and maintainability reviews.",
                prompt="""You are a code review specialist with expertise in security, performance, and best practices.
Identify security vulnerabilities, check for performance issues, and suggest specific improvements.""",
                tools=["Read", "Grep", "Glob"],
                model="sonnet",
            ),
        },
    ),
):
    if hasattr(message, "result"):
        print(message.result)
```

## `AgentDefinition` Fields

| Field | Required | Description |
|-------|----------|-------------|
| `description` | Yes | When Claude should invoke this agent. Write this carefully — it's how Claude decides to delegate. |
| `prompt` | Yes | The agent's system prompt. Defines its expertise and constraints. |
| `tools` | No | Allowed tools. Omit to inherit all tools from parent. |
| `disallowedTools` | No | Tools to remove. |
| `model` | No | Model override (`"sonnet"`, `"opus"`, `"haiku"`, or full model ID). |
| `maxTurns` | No | Maximum agentic turns. |
| `mcpServers` | No | MCP servers by name or inline config. |
| `background` | No | Run as non-blocking background task. |
| `permissionMode` | No | Permission mode for this subagent. |

**Subagents cannot spawn their own subagents.** Don't include `Agent` in a subagent's `tools` array.

## What Subagents Receive

The only channel from parent to subagent is the **Agent tool's prompt string**. Include any file paths, error messages, or decisions the subagent needs directly in that string.

The subagent also receives its own system prompt, project CLAUDE.md, and its tool definitions. It does not receive the parent's conversation history or the parent's system prompt.

## Invoking Subagents

**Automatic**: Claude reads each subagent's `description` and delegates matching tasks automatically. Write descriptions that specify when — not just what — the agent does.

**Explicit**: mention the subagent by name in your prompt:
```
"Use the code-reviewer agent to check the authentication module"
```

## Detecting Invocation in the Message Stream

Look for `tool_use` blocks where `name == "Agent"` (older SDK versions used `"Task"`). Messages from within a subagent include `parent_tool_use_id`.

```python
if isinstance(block, ToolUseBlock) and block.name in ("Task", "Agent"):
    print(f"Subagent invoked: {block.input.get('subagent_type')}")
if hasattr(message, "parent_tool_use_id") and message.parent_tool_use_id:
    print("  (running inside subagent)")
```

## Resuming Subagents

1. Capture `session_id` from the first query's `ResultMessage`
2. Extract `agentId` from message content (appears as `agentId: <uuid>` in Agent tool results)
3. Resume: pass `resume=session_id` and include the agent ID in the prompt

Both the session and the agent definition must be the same as the original run.

## Dynamic Agent Configuration

```python
def create_security_agent(security_level: str) -> AgentDefinition:
    is_strict = security_level == "strict"
    return AgentDefinition(
        description="Security code reviewer",
        prompt=f"You are a {'strict' if is_strict else 'balanced'} security reviewer...",
        tools=["Read", "Grep", "Glob"],
        model="opus" if is_strict else "sonnet",
    )
```

## Related

- [[../overview/agent-sdk-overview]]
- [[../sessions/sessions]] — subagent transcripts persist within their session

## Sources

- [[../../raw/subagents/index|Agent SDK: Subagents in the SDK]] — Anthropic, 2025.
