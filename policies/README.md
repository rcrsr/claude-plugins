# Policies Plugin

Plugin wrapper for mcp-policy-server. Auto-fetches policies for subagents via PreToolUse hook.

## How It Works

When a `Task` tool is invoked, this hook extracts § references from the subagent's `## Required Policies` section and injects matching policy content into the prompt.

## Setup

1. Create policy files in `.claude/policies/*.md` using § notation
2. Reference policies in your subagent files using `## Required Policies` section with JSON array

## Commands

| Command | Description |
| ------- | ----------- |
| `/policies:create-policies` | Extract patterns from existing code to create policy documents |
| `/policies:review-policies` | Audit all policies against §META standards in parallel |

## Agents

| Agent | Description |
| ----- | ----------- |
| `policy-engineer` | Creates, edits, and optimizes policy documents following §META standards |
| `policy-reviewer` | Reviews policies against §META; reports violations with line numbers |

## Bundled Policies

| Policy | Description |
| ------ | ----------- |
| `policies-meta.md` | Standards for policy document structure and effective prompting |

### §META Highlights

- **Size limits:** 500 lines/doc, 50 lines/section, 10 lines/example
- **Content rules:** Be explicit, lead with context, show correct/incorrect pairs, use tables
- **Structure:** Title, intro, TOC, `{§PREFIX.X}` headers, `{§END}` marker

## Dependencies

Requires [@rcrsr/mcp-policy-server](https://github.com/rcrsr/mcp-policy-server). See that repository for:

- Policy file format and § notation syntax
- Configuration options
- Troubleshooting
