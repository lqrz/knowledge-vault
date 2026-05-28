# Configure Permissions

> Source: https://code.claude.com/docs/en/agent-sdk/permissions

## Permission Evaluation Order

1. **Hooks** — run first; can deny or pass on
2. **Deny rules** (`disallowedTools`) — bare-name removes from context; scoped blocks matching calls
3. **Permission mode** — `bypassPermissions` approves everything; `acceptEdits` approves file ops
4. **Allow rules** (`allowedTools`) — listed tools are approved
5. **`canUseTool` callback** — if nothing resolved it; skipped in `dontAsk` mode

## Allow and Deny Rules

| Option | Effect |
|--------|--------|
| `allowed_tools=["Read", "Grep"]` | Auto-approves listed tools; others fall through |
| `disallowed_tools=["Bash"]` | Removes `Bash` from context entirely |
| `disallowed_tools=["Bash(rm *)"]` | Leaves `Bash` in context; denies `rm *` calls |

**Warning:** `allowed_tools` does not constrain `bypassPermissions`. If you need to block tools in bypass mode, use `disallowed_tools`.

For locked-down agents, pair `allowedTools` with `permissionMode: "dontAsk"`:
```typescript
{ allowedTools: ["Read", "Glob", "Grep"], permissionMode: "dontAsk" }
```

Rules can also be set in `.claude/settings.json` (loaded when `project` source is enabled).

## Permission Modes

| Mode | Tool behavior |
|------|---------------|
| `default` | No auto-approvals; unmatched tools trigger `canUseTool` callback |
| `dontAsk` | Anything not pre-approved is denied; `canUseTool` never called |
| `acceptEdits` | File edits and filesystem ops auto-approved |
| `bypassPermissions` | All tools run without prompts (use with caution) |
| `plan` | Read-only tools only; Claude analyzes without editing |
| `auto` (TS only) | Model classifier approves/denies each call |

**Subagent inheritance:** When parent uses `bypassPermissions`, `acceptEdits`, or `auto`, all subagents inherit that mode and it cannot be overridden.

## `acceptEdits` Auto-Approved Operations

- File edits (Edit, Write tools)
- Filesystem commands: `mkdir`, `touch`, `rm`, `rmdir`, `mv`, `cp`, `sed`

Only within the working directory or `additionalDirectories`. Paths outside scope still prompt.

## Setting Permission Mode Dynamically

```python
# Python: set at query time
ClaudeAgentOptions(permission_mode="acceptEdits")

# Python: change mid-session
await client.set_permission_mode("acceptEdits")
```

```typescript
// TypeScript: set at query time
options: { permissionMode: "acceptEdits" }

// TypeScript: change mid-session
await q.setPermissionMode("acceptEdits");
```
