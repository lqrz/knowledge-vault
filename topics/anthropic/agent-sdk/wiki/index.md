# Anthropic Agent SDK

Wiki index for the Claude Agent SDK. Sources: https://code.claude.com/docs/en/agent-sdk/

## Overview and Getting Started

- [[overview/agent-sdk-overview]] — what the SDK is, installation, `query()`, built-in tools, comparison with other Claude products
- [[overview/agent-loop]] — message types, content blocks, and how the loop detects tool calls (with diagram)

## Custom Tools

- [[custom-tools/defining-tools]] — `@tool` decorator (Python), `tool()` helper (TypeScript), input schemas, handlers, annotations
- [[custom-tools/tool-access-control]] — tool name format, availability vs. permission layers, `allowedTools`, `disallowedTools`
- [[custom-tools/error-handling]] — throwing vs. returning `isError: true`, keeping the agent loop alive
- [[custom-tools/returning-content]] — text, image, and resource blocks; `structuredContent`

## MCP

- [[mcp/mcp]] — transport types (stdio, HTTP/SSE, SDK servers), tool naming, authentication, error handling

## Tool Search

- [[tool-search/tool-search]] — deferring tool definition loading for large catalogs; `ENABLE_TOOL_SEARCH` config

## Permissions

- [[permissions/permissions]] — evaluation order, allow/deny rules, permission modes (`default`, `dontAsk`, `acceptEdits`, `bypassPermissions`, `plan`, `auto`)

## Hooks

- [[hooks/hooks]] — `PreToolUse`, `PostToolUse`, `Notification`, and other events; matchers; callback inputs/outputs; blocking, modifying, async side effects

## Sessions

- [[sessions/sessions]] — continue, resume, fork; `ClaudeSDKClient`; session IDs; cross-host considerations

## Subagents

- [[subagents/subagents]] — `AgentDefinition`, context isolation, parallelization, invocation, detecting in message stream

## User Input

- [[user-input/user-input]] — `canUseTool` callback; tool approvals; `AskUserQuestion` clarifying questions; option previews
