# Subagents in the SDK

> Source: https://code.claude.com/docs/en/agent-sdk/subagents

Subagents are separate agent instances your main agent can spawn to handle focused subtasks. Benefits: context isolation (intermediate tool calls stay inside subagent), parallelization, specialized instructions, and restricted tool access.

## Three Ways to Create Subagents

1. **Programmatically** — `agents` parameter in `query()` options (recommended for SDK)
2. **Filesystem-based** — markdown files in `.claude/agents/` directories
3. **Built-in general-purpose** — Claude invokes automatically via Agent tool

## Programmatic Definition

```python
from claude_agent_sdk import AgentDefinition

async for message in query(
    prompt="Review the authentication module for security issues",
    options=ClaudeAgentOptions(
        allowed_tools=["Read", "Grep", "Glob", "Agent"],  # Agent needed for subagent invocation
        agents={
            "code-reviewer": AgentDefinition(
                description="Expert code review specialist. Use for quality, security, and maintainability reviews.",
                prompt="You are a code review specialist...",
                tools=["Read", "Grep", "Glob"],
                model="sonnet",
            ),
        },
    ),
):
    ...
```

```typescript
options: {
  allowedTools: ["Read", "Grep", "Glob", "Agent"],
  agents: {
    "code-reviewer": {
      description: "Expert code review specialist...",
      prompt: "You are a code review specialist...",
      tools: ["Read", "Grep", "Glob"],
      model: "sonnet"
    }
  }
}
```

## `AgentDefinition` Fields

| Field | Required | Description |
|-------|----------|-------------|
| `description` | Yes | When to use this agent (Claude reads this) |
| `prompt` | Yes | The agent's system prompt |
| `tools` | No | Allowed tools (omit to inherit all) |
| `disallowedTools` | No | Tools to remove |
| `model` | No | Override model (`"sonnet"`, `"opus"`, `"haiku"`, full ID) |
| `skills` | No | Skills to preload |
| `mcpServers` | No | MCP servers by name or inline config |
| `maxTurns` | No | Max agentic turns |
| `background` | No | Run as non-blocking background task |
| `permissionMode` | No | Permission mode for this agent |

**Note:** Subagents cannot spawn their own subagents — don't include `Agent` in a subagent's `tools`.

## What Subagents Inherit

| Receives | Does not receive |
|---------|-----------------|
| Its own system prompt + Agent tool prompt | Parent's conversation history or tool results |
| Project CLAUDE.md | Preloaded skill content (unless in `skills`) |
| Tool definitions (inherited or subset) | Parent's system prompt |

The only channel from parent to subagent is the Agent tool's prompt string. Include needed file paths, error messages, or decisions directly in that prompt.

## Invocation

**Automatic**: Claude matches task to subagent based on `description`. Write clear, specific descriptions.

**Explicit**: mention by name in prompt: `"Use the code-reviewer agent to check the authentication module"`

## Detecting Subagent Invocation

Check for `tool_use` blocks where `name == "Agent"` (or `"Task"` for older SDK versions). Messages from inside a subagent include `parent_tool_use_id`.

## Resuming Subagents

1. Capture `session_id` from the first query
2. Extract `agentId` from message content (search for `agentId: <uuid>`)
3. Resume: `options: { resume: sessionId }` + include agent ID in prompt

Must resume the same session. Must pass same agent definition in `agents` parameter.

## Common Tool Combinations

| Use case | Tools |
|----------|-------|
| Read-only analysis | `Read`, `Grep`, `Glob` |
| Test execution | `Bash`, `Read`, `Grep` |
| Code modification | `Read`, `Edit`, `Write`, `Grep`, `Glob` |
| Full access | Omit `tools` field (inherits all) |
