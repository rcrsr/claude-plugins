# Policies Plugin

Auto-fetch policies for subagents via PreToolUse hook.

## How It Works

When a `Task` tool is invoked, this hook extracts § references from the subagent definition and injects matching policy content into the prompt.

## Setup

1. Create policy files in `.claude/policies/*.md` using § notation
2. Reference policies in your subagent files (e.g., `Required: §DESIGN.1, §DESIGN.2`)

## Dependencies

Requires [@rcrsr/mcp-policy-server](https://github.com/rcrsr/mcp-policy-server). See that repository for:

- Policy file format and § notation syntax
- Configuration options
- Troubleshooting
