# Hooks

Hooks are async callback functions that run at defined points in the agent lifecycle. They can block tool calls, modify inputs and outputs, inject context into the conversation, or perform side effects like logging and notifications.

## When to Use Hooks vs. Other Controls

Use **hooks** when you need runtime logic: file path validation, external API checks, dynamic blocking based on the content of a specific call, or asynchronous side effects.

Use **`allowedTools` / `disallowedTools`** when your rules are static: always allow `Read`, always block `Bash`.

Use **`canUseTool` callback** for interactive human approval at runtime.

## Available Events

Key events available in both Python and TypeScript:

- `PreToolUse` — before a tool executes; can block or modify
- `PostToolUse` — after a tool returns; can augment or replace the result
- `PostToolUseFailure` — after a tool fails
- `UserPromptSubmit` — when a user prompt is submitted; can inject context
- `Stop` — when the agent finishes
- `SubagentStart` / `SubagentStop` — subagent lifecycle
- `PreCompact` — before conversation compaction
- `PermissionRequest` — when a permission dialog would appear
- `Notification` — agent status messages (useful for Slack/webhook forwarding)

TypeScript-only: `PostToolBatch`, `MessageDisplay`, `SessionStart`, `SessionEnd`, `Setup`, `TeammateIdle`, `TaskCompleted`, `ConfigChange`, `WorktreeCreate`, `WorktreeRemove`.

## Basic Setup

```python
from claude_agent_sdk import ClaudeAgentOptions, HookMatcher

async def my_hook(input_data, tool_use_id, context):
    # inspect input_data, return a decision
    return {}  # empty = allow without changes

options = ClaudeAgentOptions(
    hooks={
        "PreToolUse": [HookMatcher(matcher="Write|Edit", hooks=[my_hook])]
    }
)
```

```typescript
import { query, HookCallback } from "@anthropic-ai/claude-agent-sdk";

const myHook: HookCallback = async (input, toolUseID, { signal }) => {
  return {};  // empty = allow without changes
};

options: {
  hooks: {
    PreToolUse: [{ matcher: "Write|Edit", hooks: [myHook] }]
  }
}
```

## Matchers

The `matcher` field is a regex matched against the tool name (for tool hooks). Omit it to match every occurrence of the event.

```python
HookMatcher(matcher="Write|Edit", hooks=[...])      # file-write tools only
HookMatcher(matcher="^mcp__", hooks=[...])           # all MCP tools
HookMatcher(matcher="Bash", hooks=[...])             # Bash only
HookMatcher(hooks=[...])                             # everything (no matcher)
```

Matchers do **not** filter by file path or other arguments — do that inside the callback by checking `tool_input`.

## Callback Return Values

Return `{}` to pass through. Otherwise include:

**Top-level fields** (any event):
- `systemMessage` — shows a message to the user (not the model)
- `continue` / `continue_` — whether the agent keeps running after this hook

**`hookSpecificOutput`** (controls the current operation):
- `hookEventName` — required in the output to identify the hook type
- `permissionDecision` — `"allow"`, `"deny"`, `"ask"`, or `"defer"`
- `permissionDecisionReason` — sent to the model explaining why
- `updatedInput` — modified tool arguments (requires `permissionDecision: "allow"`)
- `additionalContext` — appended to tool result (`PostToolUse`)
- `updatedToolOutput` — replaces tool result entirely (`PostToolUse`)

When multiple hooks apply, deny overrides all others.

## Blocking an Operation

```python
async def protect_env_files(input_data, tool_use_id, context):
    file_path = input_data["tool_input"].get("file_path", "")
    if file_path.endswith(".env"):
        return {
            "hookSpecificOutput": {
                "hookEventName": input_data["hook_event_name"],
                "permissionDecision": "deny",
                "permissionDecisionReason": "Cannot modify .env files",
            }
        }
    return {}
```

## Modifying Tool Input

```python
async def redirect_to_sandbox(input_data, tool_use_id, context):
    if input_data["tool_name"] == "Write":
        original_path = input_data["tool_input"].get("file_path", "")
        return {
            "hookSpecificOutput": {
                "hookEventName": input_data["hook_event_name"],
                "permissionDecision": "allow",
                "updatedInput": {
                    **input_data["tool_input"],
                    "file_path": f"/sandbox{original_path}",
                },
            }
        }
    return {}
```

## Async Side-Effect Hooks

For logging/webhook hooks that don't need to influence the agent:

```python
asyncio.create_task(send_to_logging_service(input_data))
return {"async_": True, "asyncTimeout": 30000}
```

The agent continues immediately without waiting. Cannot block, modify, or inject context.

## Notification Hooks (Slack/Webhooks)

Use `Notification` hooks to receive agent status updates and forward them externally. Notification types: `permission_prompt`, `idle_prompt`, `auth_success`, `elicitation_dialog`, `elicitation_response`, `elicitation_complete`.

```typescript
const notificationHandler: HookCallback = async (input, toolUseID, { signal }) => {
  await fetch("https://hooks.slack.com/services/YOUR/WEBHOOK/URL", {
    method: "POST",
    body: JSON.stringify({ text: `Agent status: ${(input as any).message}` }),
    signal
  });
  return {};
};
```

## Python Note: `SessionStart` / `SessionEnd`

Not available as SDK callback hooks in Python. Use shell command hooks in `.claude/settings.json` loaded via `setting_sources=["project"]`.

## Related

- [[../permissions/permissions]] — permission evaluation runs after hooks
- [[../custom-tools/defining-tools]]

## Sources

- [[../../raw/hooks/index|Agent SDK: Intercept and Control Agent Behavior with Hooks]] — Anthropic, 2025.
