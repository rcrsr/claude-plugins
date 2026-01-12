# Claude Code Plugins

Plugins that make Claude Code more reliable. Catch errors early, debug failures, enforce standards.

## Installation

```bash
# Add the marketplace
/plugin marketplace add rcrsr/claude-plugins

# Install any plugin
/plugin install <plugin>@rcrsr
```

## Available Plugins

### [checker](https://github.com/rcrsr/checker)

**Automated code quality checks on every edit.**

Runs your linters, formatters, and type checkers after every file change. Errors block until fixed. Supports monorepos with different tools per directory. Zero-config start with `/checker:create`.

```bash
/plugin install checker@rcrsr
```

---

### [snoop](https://github.com/rcrsr/snoop)

**Debug Claude Code sessions with transcript capture.**

Captures every session as streamlined JSONL. Tracks ESC interrupts, tool usage, and timing. Built-in reviewer identifies anti-patterns like loops, scope creep, and redundant reads.

```bash
/plugin install snoop@rcrsr
```

---

### [policies](./policies)

**Auto-inject policies into subagents.**

Codify your team's standards once, reference them everywhere with § notation. Subagents declare what policies they need; the hook fetches and injects them automatically.

```bash
/plugin install policies@rcrsr
```
