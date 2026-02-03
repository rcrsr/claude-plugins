# velocity

Velocity and quality assessment for softwareengineering teams.

## Why velocity?

**Data-driven team insights.** Measure commits/day, contributor distribution, and code quality in one report. No manual data gathering.

**Risk visibility.** Bus factor, Gini coefficient, and ownership concentration reveal team health risks before they become problems.

**Executive-ready output.** Three-agent workflow produces reports with metrics, thresholds, and actionable recommendations.

## Installation

```bash
# From marketplace (bundled in claude-plugins)
/plugin marketplace add rcrsr/claude-plugins
/plugin install velocity@rcrsr

# Or load locally
claude --plugin-dir /path/to/claude-plugins/velocity
```

## Quick Start

1. Run `/velocity:velocity-assessment` in any git repository
2. Agents analyze git history, code quality, and compile findings
3. Receive executive report with risk scores and recommendations

## Skills

| Skill | Description |
|-------|-------------|
| `/velocity:velocity-assessment` | Generate executive velocity report (args: `days=90`, `branch=main`) |

## Agents

| Agent | Description |
|-------|-------------|
| `velocity-git-analyst` | Git commit and contributor analysis |
| `velocity-quality-assessor` | Code quality assessment via Codanna CLI |
| `velocity-reporter` | Executive report compilation (uses opus) |

## Metrics Quick Reference

| Category | Key Metrics |
|----------|-------------|
| Velocity | Commits/day, lines/commit, release frequency |
| Contributor Health | Bus factor, Gini coefficient, file ownership |
| Code Quality | Error handling, complexity hotspots, test ratio |

## Prerequisites

- Git repository with commit history
- [Codanna](https://docs.codanna.sh/installation) for code quality analysis (Phase 2 skipped if unavailable)
