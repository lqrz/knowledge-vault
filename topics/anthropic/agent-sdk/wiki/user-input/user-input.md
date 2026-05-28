# User Input and Approvals

The `canUseTool` callback is the SDK's mechanism for surfacing Claude's tool requests and clarifying questions to your users. It pauses agent execution and gives your application control over whether and how to proceed.

## When Claude Asks for Input

Claude requests user input in two situations:

1. **Tool approval**: Claude wants to use a tool that isn't pre-approved by `allowedTools` or permission rules.
2. **Clarifying question**: Claude calls the `AskUserQuestion` tool to ask about a decision with multiple valid approaches.

Both trigger the `canUseTool` callback. Check `tool_name == "AskUserQuestion"` to distinguish the two cases.

## Setting Up the Callback

```python
async def my_callback(tool_name: str, input_data: dict, context) -> PermissionResultAllow | PermissionResultDeny:
    if tool_name == "AskUserQuestion":
        return await handle_question(input_data)
    return await prompt_user_for_approval(tool_name, input_data)

options = ClaudeAgentOptions(can_use_tool=my_callback)
```

**Python note:** `can_use_tool` requires streaming mode and a dummy `PreToolUse` hook returning `{"continue_": True}` to keep the stream open. Without this hook, the stream closes before the callback can be invoked.

```python
async def dummy_hook(input_data, tool_use_id, context):
    return {"continue_": True}

options = ClaudeAgentOptions(
    can_use_tool=my_callback,
    hooks={"PreToolUse": [HookMatcher(matcher=None, hooks=[dummy_hook])]},
)
```

## Responding to Tool Approval Requests

| Response | Python | TypeScript |
|----------|--------|------------|
| Allow | `PermissionResultAllow(updated_input=input_data)` | `{ behavior: "allow", updatedInput: input }` |
| Deny | `PermissionResultDeny(message="reason")` | `{ behavior: "deny", message: "reason" }` |

The `message` in a denial is sent to Claude, which may adjust its approach based on the explanation.

**Approving with modified input** (e.g., redirect file paths to a sandbox):
```python
return PermissionResultAllow(
    updated_input={**input_data, "file_path": f"/sandbox{input_data['file_path']}"}
)
```

## Handling Clarifying Questions

When `tool_name == "AskUserQuestion"`, the input contains Claude's questions as multiple-choice options:

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

Collect the user's selection and return an `answers` object keyed by the `question` field, with the selected option's `label` as the value. For multi-select, join with `", "`:

```python
return PermissionResultAllow(
    updated_input={
        "questions": input_data["questions"],
        "answers": {
            "How should I format the output?": "Summary",
        },
    }
)
```

If the user types their own answer (free text, not a listed option), use that text directly as the value.

## TypeScript: Option Previews

Set `toolConfig.askUserQuestion.previewFormat` to `"markdown"` or `"html"` to receive a `preview` field on each option. Claude includes previews where visual comparison helps (layout choices, UI mockups). Check for `undefined` before rendering — Claude omits previews for text-only choices.

## Clarifying Questions and `plan` Mode

Clarifying questions are most common in `plan` mode, where Claude explores the codebase and gathers requirements before proposing changes. This makes `plan` mode well-suited for interactive workflows.

If you restrict Claude's tool set with a `tools` array, include `AskUserQuestion` explicitly or Claude won't be able to ask questions:

```python
ClaudeAgentOptions(tools=["Read", "Glob", "Grep", "AskUserQuestion"], can_use_tool=my_callback)
```

## Limitations

- `AskUserQuestion` is not available inside subagents
- 1-4 questions per call, 2-4 options each

## Alternatives

Use **hooks** when you want to allow/deny tools automatically based on logic, without user interaction.

Use **streaming input** to interrupt the agent mid-task or provide additional context without waiting for a callback.

Use **custom tools** to build structured approval workflows beyond the multiple-choice format `AskUserQuestion` provides.

## Related

- [[../permissions/permissions]]
- [[../hooks/hooks]]
- [[../custom-tools/defining-tools]]

## Sources

- [[../../raw/user-input/index|Agent SDK: Handle Approvals and User Input]] — Anthropic, 2025.
