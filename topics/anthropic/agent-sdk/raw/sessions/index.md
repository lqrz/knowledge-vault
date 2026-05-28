# Work with Sessions

> Source: https://code.claude.com/docs/en/agent-sdk/sessions

A session is the conversation history the SDK accumulates: prompts, tool calls, tool results, and responses. Written to disk automatically. Sessions persist the **conversation**, not the filesystem (use file checkpointing for that).

## Choosing an Approach

| What you're building | What to use |
|---------------------|-------------|
| One-shot task | Nothing extra. One `query()` call. |
| Multi-turn chat in one process | `ClaudeSDKClient` (Python) or `continue: true` (TypeScript) |
| Pick up after process restart | `continue_conversation=True` / `continue: true` |
| Resume a specific past session | Capture session ID, pass to `resume` |
| Try alternative without losing original | Fork the session |
| Stateless (TypeScript only) | `persistSession: false` |

## Continue vs Resume vs Fork

- **Continue**: finds the most recent session in current directory. No ID tracking.
- **Resume**: takes a specific session ID. Required for multiple sessions or non-latest.
- **Fork**: creates a new session starting with a copy of original history. Original unchanged.

## Python: `ClaudeSDKClient`

Handles session IDs internally. Each `client.query()` continues the same session automatically.

```python
async with ClaudeSDKClient(options=options) as client:
    await client.query("Analyze the auth module")
    async for message in client.receive_response():
        print(message)

    await client.query("Now refactor it to use JWT")  # same session
    async for message in client.receive_response():
        print(message)
```

## TypeScript: `continue: true`

```typescript
// First query: creates new session
for await (const message of query({ prompt: "Analyze the auth module", options: {...} })) { ... }

// Second query: resumes most recent session
for await (const message of query({
  prompt: "Now refactor it to use JWT",
  options: { continue: true, allowedTools: [...] }
})) { ... }
```

## Capture Session ID

Session ID is on `ResultMessage.session_id` (always present). In TypeScript, also available earlier on the init `SystemMessage`.

```python
if isinstance(message, ResultMessage):
    session_id = message.session_id
```

## Resume by ID

```python
async for message in query(
    prompt="Now implement the refactoring you suggested",
    options=ClaudeAgentOptions(resume=session_id, allowed_tools=[...]),
):
    ...
```

**If resume returns a fresh session**: mismatched `cwd`. Sessions stored at `~/.claude/projects/<encoded-cwd>/*.jsonl`. Must match exactly.

## Fork to Explore Alternatives

```python
# Fork: creates new session branching from session_id
async for message in query(
    prompt="Instead of JWT, implement OAuth2",
    options=ClaudeAgentOptions(resume=session_id, fork_session=True),
):
    if isinstance(message, ResultMessage):
        forked_id = message.session_id
```

Fork branches conversation history, not filesystem. File changes are real and shared.

## Resume Across Hosts

Sessions are local. Options:
1. Copy `~/.claude/projects/<encoded-cwd>/<session-id>.jsonl` to the same path on new host
2. Skip resume — pass prior results in a fresh session's prompt (often more robust)

## Session Management Functions

- Python: `list_sessions()`, `get_session_messages()`, `get_session_info()`, `rename_session()`, `tag_session()`
- TypeScript: `listSessions()`, `getSessionMessages()`, `getSessionInfo()`, `renameSession()`, `tagSession()`
