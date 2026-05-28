# Sessions

A session is the conversation history the Agent SDK accumulates during a `query()` run: prompts, tool calls, tool results, and responses. The SDK writes it to disk as a JSONL file so you can return to it later.

Sessions persist the **conversation**, not the filesystem state. To snapshot and revert file changes, use file checkpointing.

## When You Need Session Management

Within a single `query()` call, the agent handles as many turns as needed internally — no session management required. Session management only becomes relevant when sending **multiple prompts** that need shared context across separate calls.

## Continue, Resume, Fork

**Continue** — finds the most recent session in the current working directory. No ID needed. Use for single-conversation applications.

**Resume** — takes a specific session ID. Required when you have multiple sessions or want to return to something other than the most recent.

**Fork** — creates a new session starting with a copy of the original history. The original is unchanged. Use to explore an alternative approach while keeping the original thread.

## Python: `ClaudeSDKClient`

The recommended way to do multi-turn conversations in Python. Handles session IDs internally.

```python
async with ClaudeSDKClient(options=options) as client:
    await client.query("Analyze the auth module")
    async for message in client.receive_response():
        print(message)

    # Automatically continues the same session — no ID tracking
    await client.query("Now refactor it to use JWT")
    async for message in client.receive_response():
        print(message)
```

## TypeScript: `continue: true`

Pass `continue: true` on each subsequent call to resume the most recent session on disk.

```typescript
// First call: creates a new session
for await (const message of query({ prompt: "Analyze auth module", options: {...} })) { ... }

// Second call: resumes most recent session
for await (const message of query({
  prompt: "Now refactor to use JWT",
  options: { continue: true, allowedTools: [...] }
})) { ... }
```

## Capturing the Session ID

Read `session_id` from any `ResultMessage`. In TypeScript, also available on the init `SystemMessage`.

```python
if isinstance(message, ResultMessage):
    session_id = message.session_id
```

## Resume by ID

```python
async for message in query(
    prompt="Implement the refactoring you suggested",
    options=ClaudeAgentOptions(resume=session_id, allowed_tools=[...]),
):
    ...
```

**Common pitfall:** if `resume` returns a fresh session, the `cwd` doesn't match. Sessions are stored at `~/.claude/projects/<encoded-cwd>/<session-id>.jsonl` where the directory path is encoded by replacing non-alphanumeric characters with `-`. The resume call must run from the same working directory.

## Forking

```python
forked_id = None
async for message in query(
    prompt="Instead of JWT, implement OAuth2",
    options=ClaudeAgentOptions(resume=session_id, fork_session=True),
):
    if isinstance(message, ResultMessage):
        forked_id = message.session_id  # different from session_id
```

After forking, `session_id` and `forked_id` are two independent sessions you can resume separately. Forking branches the conversation history, not the filesystem — file edits from a forked session are real and visible to any session in the same directory.

## Cross-Host Resume

Sessions are local. Two options:
1. Copy the JSONL transcript file from `~/.claude/projects/...` to the same path on the target host
2. Skip `resume` — extract the results you need from the first run and pass them in a new session's prompt

## Session Utilities

Both SDKs provide functions for managing sessions on disk:

- `list_sessions()` / `listSessions()` — enumerate sessions
- `get_session_messages()` / `getSessionMessages()` — read messages from a session
- `get_session_info()` / `getSessionInfo()` — metadata about a session
- `rename_session()` / `renameSession()` — give a session a human-readable title
- `tag_session()` / `tagSession()` — organize sessions by tag

## Related

- [[../overview/agent-sdk-overview]]
- [[../hooks/hooks]]

## Sources

- [[../../raw/sessions/index|Agent SDK: Work with Sessions]] — Anthropic, 2025.
