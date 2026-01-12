# policies

Auto-fetches policy content for subagents. Codify your team's standards once, enforce them everywhere.

## Why policies?

**Consistent agent behavior.** Subagents automatically receive relevant policies. No manual context stuffing. No forgotten guidelines.

**Reusable standards.** Write coding standards, architecture rules, or review criteria once. Reference them with § notation across all your agents.

**Self-maintaining docs.** Built-in commands create policies from existing code patterns and audit them against §META standards.

**Scoped injection.** Policies load only when needed. Agents declare what they require; the hook fetches exactly that.

## Installation

```bash
# From marketplace (bundled in claude-plugins)
/plugin marketplace add rcrsr/claude-plugins
/plugin install policies@rcrsr

# Or load locally
claude --plugin-dir /path/to/claude-plugins/policies
```

## Quick Start

1. Create policy files in `.claude/policies/*.md` using § notation
2. Add `## Required Policies` section to your subagent with JSON array of § references
3. When the agent runs, policies are auto-injected

## Commands

| Command | Description |
|---------|-------------|
| `/policies:create-policies` | Extract patterns from existing code to create policy documents |
| `/policies:review-policies` | Audit all policies against §META standards |

## Agents

| Agent | Description |
|-------|-------------|
| `policy-engineer` | Creates and optimizes policy documents |
| `policy-reviewer` | Audits policies, reports violations with line numbers |

## Bundled Policies

| Policy | Description |
|--------|-------------|
| `policies-meta.md` | Standards for policy document structure (§META) |

### §META Quick Reference

- **Size limits:** 500 lines/doc, 50 lines/section, 10 lines/example
- **Structure:** Title, intro, TOC, `{§PREFIX.X}` headers, `{§END}` marker
- **Content:** Be explicit, lead with context, show correct/incorrect pairs

## Dependencies

Requires [@rcrsr/mcp-policy-server](https://github.com/rcrsr/mcp-policy-server) for § notation parsing.
