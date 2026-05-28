# Handle Approvals and User Input

> Source: https://code.claude.com/docs/en/agent-sdk/user-input

Claude requests user input in two situations: when it needs **permission to use a tool** and when it has **clarifying questions** (via `AskUserQuestion`). Both trigger the `canUseTool` callback.

## `canUseTool` Callback

```python
async def handle_tool_request(tool_name, input_data, context):
    # return allow or deny
    ...

options = ClaudeAgentOptions(can_use_tool=handle_tool_request)
```

Fires for:
1. **Tool needs approval** — tool not auto-approved by permission rules
2. **Claude asks a question** — `tool_name == "AskUserQuestion"`

## Callback Arguments

| Argument | Description |
|----------|-------------|
| `toolName` | Tool name (e.g., `"Bash"`, `"Write"`, `"AskUserQuestion"`) |
| `input` | Tool-specific parameters |
| `context` | Optional `suggestions` (PermissionUpdate entries) + cancellation signal |

Common input fields: `command`/`description`/`timeout` (Bash), `file_path`/`content` (Write), `file_path`/`old_string`/`new_string` (Edit).

## Response Types

| Response | Python | TypeScript |
|----------|--------|------------|
| Allow | `PermissionResultAllow(updated_input=...)` | `{ behavior: "allow", updatedInput }` |
| Deny | `PermissionResultDeny(message=...)` | `{ behavior: "deny", message }` |

Options beyond allow/deny:
- **Approve with changes**: modify input before execution
- **Suggest alternative**: deny + message guiding Claude toward what user wants
- **Redirect**: use streaming input to send entirely new instruction

## `AskUserQuestion` Tool

Claude generates multiple-choice questions when it needs direction. Input structure:

```json
{
  "questions": [
    {
      "question": "How should I format the output?",
      "header": "Format",
      "options": [
        { "label": "Summary", "description": "Brief overview" },
        { "label": "Detailed", "description": "Full explanation" }
      ],
      "multiSelect": false
    }
  ]
}
```

Common in `plan` mode. Include `AskUserQuestion` in your `tools` array if restricting Claude's tool set.

## Responding to Clarifying Questions

Return `answers` object: keys are `question` text, values are selected option `label`:

```python
return PermissionResultAllow(
    updated_input={
        "questions": input_data.get("questions", []),
        "answers": {
            "How should I format the output?": "Summary",
            "Which sections should I include?": "Introduction, Conclusion",  # multi-select: comma-join
        },
    }
)
```

For free-text: display an "Other" option, use the custom text directly as the answer value.

## TypeScript: Option Previews

Set `toolConfig.askUserQuestion.previewFormat` to `"markdown"` or `"html"` to get a `preview` field on each option. Claude includes previews where visual comparison helps and omits them where it doesn't. Check for `undefined` before rendering.

## Python: Required Workaround

`can_use_tool` requires streaming mode and a `PreToolUse` hook returning `{"continue_": True}` to keep the stream open:

```python
async def dummy_hook(input_data, tool_use_id, context):
    return {"continue_": True}

options = ClaudeAgentOptions(
    can_use_tool=can_use_tool,
    hooks={"PreToolUse": [HookMatcher(matcher=None, hooks=[dummy_hook])]},
)
```

## Limitations

- `AskUserQuestion` not available in subagents
- 1-4 questions per call, 2-4 options each

## Alternatives

- **Streaming input**: interrupt mid-task, provide context, build chat interfaces
- **Custom tools**: structured forms, external approval systems, domain-specific workflows
