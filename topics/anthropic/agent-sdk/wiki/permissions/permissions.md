# Permissions

The Agent SDK's permission system has two layers — availability and approval — evaluated in a fixed order. Understanding the order matters because a mistake like pairing `bypassPermissions` with `allowedTools` won't do what you expect.

## Evaluation Order

When Claude requests a tool call:

1. **Hooks** run first. A hook can deny or pass through. An `allow` from a hook doesn't skip steps 2–5.
2. **Deny rules** (`disallowedTools`). Bare-name entries remove the tool from context before this step. Scoped entries block matching calls.
3. **Permission mode** is applied. `bypassPermissions` approves everything remaining. `acceptEdits` approves file operations.
4. **Allow rules** (`allowedTools`) are checked. Listed tools are approved.
5. **`canUseTool` callback** is invoked for anything not yet resolved. In `dontAsk` mode, this step is skipped and the tool is denied.

## Allow and Deny Rules

**`allowedTools`** pre-approves specific tools so they run without a prompt. Tools not listed still exist and fall through to the permission mode and `canUseTool`. This is an approval shortcut, not an exclusion list.

**`disallowedTools` (bare name)** — removes the tool from context entirely. Claude never sees it and cannot attempt it. Strongest form of blocking.

**`disallowedTools` (scoped)** — e.g., `"Bash(rm *)"` — leaves the tool in context but blocks calls matching the pattern. Claude may still try the tool and waste a turn on the denial.

Prefer bare-name disallowing when you want Claude to never attempt a tool. Use scoped rules only for fine-grained within-tool blocking.

**Important:** `allowedTools` does not constrain `bypassPermissions`. In bypass mode, every tool is approved regardless of what you listed. To block specific tools in bypass mode, use `disallowedTools`.

## Permission Modes

| Mode | Behavior |
|------|----------|
| `default` | No auto-approvals. Unmatched tools trigger `canUseTool` callback. |
| `dontAsk` | Anything not pre-approved is silently denied. `canUseTool` never called. |
| `acceptEdits` | File edits (Edit, Write) and filesystem commands auto-approved. Other tools fall through. |
| `bypassPermissions` | All tools approved. Use only in sandboxed or fully trusted environments. |
| `plan` | Read-only tools only. Claude analyzes without modifying files. |
| `auto` (TypeScript only) | A model classifier approves or denies each tool call. |

`acceptEdits` auto-approves: Edit, Write, plus `mkdir`, `touch`, `rm`, `rmdir`, `mv`, `cp`, `sed`. Only within the working directory or `additionalDirectories`.

## Common Patterns

**Locked-down read-only agent:**
```python
ClaudeAgentOptions(
    tools=["Read", "Glob", "Grep"],   # remove all other built-ins from context
    allowed_tools=["Read", "Glob", "Grep"],
    permission_mode="dontAsk",
)
```

**MCP-only agent (no built-ins):**
```python
ClaudeAgentOptions(
    tools=[],                          # strip all built-ins
    mcp_servers={"myserver": server},
    allowed_tools=["mcp__myserver__*"],
)
```

**Block a specific command within Bash:**
```python
ClaudeAgentOptions(disallowed_tools=["Bash(rm -rf *)"])
```

## Subagent Inheritance

When the parent uses `bypassPermissions`, `acceptEdits`, or `auto`, all subagents inherit that mode. It cannot be overridden per-subagent. Since subagents can have less constrained behavior, inheriting `bypassPermissions` grants them full system access.

## Changing Mode Mid-Session

```python
# Python
await client.set_permission_mode("acceptEdits")
```
```typescript
// TypeScript
await q.setPermissionMode("acceptEdits");
```

Useful pattern: start in `default` mode, review Claude's initial approach, switch to `acceptEdits` once you trust the direction.

## Related

- [[../hooks/hooks]] — hooks run before permission rules and can block tool calls
- [[user-input]] — `canUseTool` callback for interactive approval flows
- [[../custom-tools/tool-access-control]]

## Sources

- [[../../raw/permissions/index|Agent SDK: Configure Permissions]] — Anthropic, 2025.
