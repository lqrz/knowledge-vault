# Intercept and Control Agent Behavior with Hooks

> Source: https://code.claude.com/docs/en/agent-sdk/hooks

Hooks are callback functions that run in response to agent events. Use them to block dangerous operations, log and audit tool calls, transform inputs/outputs, require human approval, or track session lifecycle.

## How Hooks Work

1. An event fires (tool call, session start, etc.)
2. SDK collects registered hooks for that event type
3. Matchers filter which hooks run
4. Callback functions execute
5. Your callback returns a decision (allow, deny, modify, inject context)

## Available Hooks

| Hook Event | Python | TypeScript | Trigger |
|------------|--------|------------|---------|
| `PreToolUse` | ✓ | ✓ | Tool call request (can block or modify) |
| `PostToolUse` | ✓ | ✓ | Tool execution result |
| `PostToolUseFailure` | ✓ | ✓ | Tool execution failure |
| `PostToolBatch` | - | ✓ | Full batch of tool calls resolves |
| `UserPromptSubmit` | ✓ | ✓ | User prompt submission |
| `MessageDisplay` | - | ✓ | Assistant message with text completes |
| `Stop` | ✓ | ✓ | Agent execution stop |
| `SubagentStart` | ✓ | ✓ | Subagent initialization |
| `SubagentStop` | ✓ | ✓ | Subagent completion |
| `PreCompact` | ✓ | ✓ | Conversation compaction request |
| `PermissionRequest` | ✓ | ✓ | Permission dialog would be displayed |
| `SessionStart` | - | ✓ | Session initialization |
| `SessionEnd` | - | ✓ | Session termination |
| `Notification` | ✓ | ✓ | Agent status messages |
| `Setup` | - | ✓ | Session setup/maintenance |
| `TeammateIdle` | - | ✓ | Teammate becomes idle |
| `TaskCompleted` | - | ✓ | Background task completes |

## Configuration

```python
options = ClaudeAgentOptions(
    hooks={
        "PreToolUse": [HookMatcher(matcher="Write|Edit", hooks=[my_callback])]
    }
)
```

```typescript
options: {
  hooks: {
    PreToolUse: [{ matcher: "Write|Edit", hooks: [myCallback] }]
  }
}
```

## Matchers

`matcher` is a regex string matched against the tool name (for tool hooks) or notification type (for `Notification` hooks).

- `"Write|Edit"` — matches Write or Edit
- `"^mcp__"` — matches all MCP tools
- omitted — matches everything

Matchers filter by **tool name only**, not by arguments. To filter by file path, check `tool_input.file_path` inside the callback.

## Callback Inputs

Every callback receives:
1. **Input data** — event-specific object (includes `session_id`, `cwd`, `hook_event_name`; `agent_id` and `agent_type` when inside a subagent)
2. **Tool use ID** — correlates `PreToolUse` / `PostToolUse` pairs
3. **Context** — `AbortSignal` in TypeScript; reserved in Python

## Callback Outputs

Return `{}` to allow without changes. Otherwise:

- **`systemMessage`** — shows a message to the user
- **`continue`** — whether agent keeps running after this hook
- **`hookSpecificOutput.permissionDecision`** — `"allow"`, `"deny"`, `"ask"`, or `"defer"`
- **`hookSpecificOutput.permissionDecisionReason`** — why (sent to model)
- **`hookSpecificOutput.updatedInput`** — modified tool input (must also set `permissionDecision: "allow"`)
- **`hookSpecificOutput.additionalContext`** — append to tool result (`PostToolUse`)
- **`hookSpecificOutput.updatedToolOutput`** — replace tool result (`PostToolUse`)

When multiple hooks apply, **deny** > **defer** > **ask** > **allow**.

## Async Output

For side-effect-only hooks (logging, webhooks), return immediately without waiting:

```python
return {"async_": True, "asyncTimeout": 30000}
```
```typescript
return { async: true, asyncTimeout: 30000 };
```

Async hooks cannot block, modify, or inject context.

## Example Patterns

**Block operation**: `permissionDecision: "deny"` + `permissionDecisionReason`

**Redirect file writes to sandbox**: check `tool_name == "Write"`, return `updatedInput` with modified path + `permissionDecision: "allow"`

**Auto-approve read-only tools**: check tool name in `["Read", "Glob", "Grep"]`, return `permissionDecision: "allow"`

**Forward notifications to Slack**: use `Notification` hook, POST `input.message` to Slack webhook

**Multiple hooks**: all matching hooks run in parallel; most restrictive result wins

## Python Note: `SessionStart` / `SessionEnd`

Not available as SDK callback hooks in Python. Use shell command hooks in `.claude/settings.json` with `setting_sources=["project"]`, or use the first message from `client.receive_response()` as a trigger.
