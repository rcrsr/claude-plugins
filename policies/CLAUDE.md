# policies

Auto-fetches policy content for subagents via PreToolUse hook. Wrapper for `@rcrsr/mcp-policy-server`.

## Development

```bash
claude --plugin-dir /path/to/claude-plugins/policies
```

## Architecture

```
hooks/
└── hooks.json               # PreToolUse binding (Task matcher)
policies/
└── policies-meta.md         # §META - policy authoring standards
agents/
├── policy-engineer.md       # Creates/edits policies per §META
└── policy-reviewer.md       # Audits policies against §META
commands/
├── create-policies.md       # /policies:create-policies
└── review-policies.md       # /policies:review-policies
```

## Hook Flow

1. PreToolUse fires on `Task` tool invocation
2. Hook reads target subagent's `## Required Policies` section
3. Extracts § references from JSON array
4. Calls `policy-fetch` to resolve and inject policy content
5. Subagent receives policies in context

## Dependencies

Requires [@rcrsr/mcp-policy-server](https://github.com/rcrsr/mcp-policy-server) for § notation parsing and policy fetching.

## Key Files

| File | Responsibility |
|------|----------------|
| `hooks/hooks.json` | PreToolUse → Task matcher, invokes policy-fetch |
| `policies/policies-meta.md` | §META standards: 500 lines/doc, 50 lines/section, structure rules |
| `agents/policy-engineer.md` | PROACTIVE agent for policy creation/editing |
| `agents/policy-reviewer.md` | Read-only auditor, reports violations with line numbers |

## §META Quick Reference

| Constraint | Limit |
|------------|-------|
| Document size | 500 lines |
| Section size | 50 lines |
| Example size | 10 lines |
| Required structure | Title, TOC, `{§PREFIX.X}` headers, `{§END}` marker |
