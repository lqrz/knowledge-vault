# Agent SDK Overview

> Source: https://code.claude.com/docs/en/agent-sdk/overview

Build AI agents that autonomously read files, run commands, search the web, edit code, and more. The Agent SDK gives you the same tools, agent loop, and context management that power Claude Code, programmable in Python and TypeScript.

## Installation

```bash
# TypeScript
npm install @anthropic-ai/claude-agent-sdk

# Python
pip install claude-agent-sdk
```

## Authentication

Set API key: `export ANTHROPIC_API_KEY=your-api-key`

Also supports:
- Amazon Bedrock: `CLAUDE_CODE_USE_BEDROCK=1`
- Claude Platform on AWS: `CLAUDE_CODE_USE_ANTHROPIC_AWS=1`
- Google Vertex AI: `CLAUDE_CODE_USE_VERTEX=1`
- Microsoft Azure: `CLAUDE_CODE_USE_FOUNDRY=1`

## Core Concepts

### Built-in Tools

| Tool | What it does |
| ---- | ------------ |
| Read | Read any file in the working directory |
| Write | Create new files |
| Edit | Make precise edits to existing files |
| Bash | Run terminal commands, scripts, git operations |
| Monitor | Watch a background script, react to output lines |
| Glob | Find files by pattern |
| Grep | Search file contents with regex |
| WebSearch | Search the web |
| WebFetch | Fetch and parse web page content |
| AskUserQuestion | Ask the user clarifying questions |

### Agent SDK vs Client SDK

Client SDK: you implement the tool loop manually. Agent SDK: Claude handles tool execution autonomously.

### Agent SDK vs Claude Code CLI

Same capabilities, different interface. CLI for interactive development; SDK for CI/CD and production automation.

### Agent SDK vs Managed Agents

| | Agent SDK | Managed Agents |
|---|---|---|
| Runs in | Your process | Anthropic-managed infrastructure |
| Interface | Python/TypeScript library | REST API |
| Agent works on | Your filesystem | A managed sandbox |
| Custom tools | In-process functions | You execute and return results |

## Claude Code Features Available

| Feature | Description | Location |
| ------- | ----------- | -------- |
| Skills | Specialized capabilities | `.claude/skills/*/SKILL.md` |
| Commands | Custom slash commands | `.claude/commands/*.md` |
| Memory | Project context | `CLAUDE.md` |
| Plugins | Extend with skills, agents, hooks, MCP servers | `plugins` option |

## Starting June 15, 2026

Agent SDK usage on subscription plans draws from a new monthly Agent SDK credit, separate from interactive usage limits.
