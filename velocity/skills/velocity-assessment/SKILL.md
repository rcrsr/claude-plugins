---
name: velocity-assessment
description: Executive velocity and quality assessment for development teams
allowed-tools: Read, Bash, Grep, Glob, Task
context: fork
---

# Executive Velocity Assessment

Assess development velocity and code quality for executive reporting.

## Usage

```
/velocity:velocity-assessment [days=90] [branch=main]
```

**Arguments:**
- `days` - Analysis window (default: 90 days)
- `branch` - Target branch (default: main)

## Three-Agent Workflow

This skill orchestrates three specialized agents sequentially:

| Phase | Agent | Purpose | Model |
|-------|-------|---------|-------|
| 1 | `velocity-git-analyst` | Git commit/contributor analysis | sonnet |
| 2 | `velocity-quality-assessor` | Code quality via Codanna CLI | sonnet |
| 3 | `velocity-reporter` | Executive report compilation | opus |

## Orchestration Instructions

### Phase 0: Codanna Preflight Check

Before launching agents, verify Codanna is initialized:

```bash
codanna config
```

If this returns configuration data, Codanna is initialized. Then check index status:

```bash
codanna mcp get_index_info --json
```

**If Codanna is not initialized or index is empty:**
- Set `$codanna_available = false`
- Skip Phase 2 (quality assessment)
- Note in final report: "Code quality assessment skipped - Codanna index not available"

**If Codanna is ready:**
- Set `$codanna_available = true`
- Proceed with all phases

### Phase 1: Git Analysis

Launch the `velocity-git-analyst` agent with:

```
Analyze git history for the past {days} days on branch {branch}.
Output JSON with velocity metrics, contributor stats, and temporal patterns.
```

Wait for completion. Store output as `$git_analysis`.

### Phase 2: Quality Assessment (Conditional)

**Skip if `$codanna_available = false`.**

If Codanna is available, launch the `velocity-quality-assessor` agent with:

```
Assess code quality using Codanna CLI (codanna mcp <tool> --json).
Output JSON with quality indicators and recommendations.
```

Wait for completion. Store output as `$quality_assessment`.

If skipped, set `$quality_assessment = null` and proceed to Phase 3.

### Phase 3: Report Compilation

Launch the `velocity-reporter` agent (model: opus) with:

```
Compile executive velocity report from:
- Git analysis: {$git_analysis}
- Quality assessment: {$quality_assessment}

Follow the report template in metrics-definitions.md and report-template.md.
```

### Output

Present the final executive report to the user.

## Reference Files

- `metrics-definitions.md` - Metric formulas and thresholds
- `report-template.md` - Executive report format

## Example Output

```
# Development Velocity Report

**Period:** 2025-11-03 to 2026-02-03 (90 days)
**Branch:** main

## Executive Summary

### Dashboard

| Metric | Value | Benchmark | Delta | Status |
|--------|-------|-----------|-------|--------|
| Commits/Day | 2.3 | 2.0 | +0.3 (15% over) | 🟢 Healthy |
| Bus Factor | 2 | 3 | -1 (33% under) | 🟡 Warning |
| Code Quality | 78% | 80% | -2pp (3% under) | 🟡 Warning |

## Contributor Breakdown

| Contributor | Commits | % of Total | Cumulative % | Role |
|-------------|---------|------------|--------------|------|
| Alice | 95 | 45.9% | 45.9% | Primary |
| Bob | 62 | 30.0% | 75.9% | Primary |
| Carol | 30 | 14.5% | 90.4% | Secondary |
| Dave | 15 | 7.2% | 97.6% | Peripheral |
| Eve | 5 | 2.4% | 100.0% | Peripheral |

### Contributor Analysis

Top 2 contributors (Alice, Bob) account for 75.9% of commits. With a bus factor of 2, the project has emerging key-person risk. Ideal distribution for 5 contributors is 20% each. Alice exceeds ideal by 26 percentage points. Knowledge transfer to Carol recommended.

## Key Findings

1. Commits/day at 2.3 exceeds benchmark by 15%
2. Bus factor of 2 falls 33% below target of 3
3. Error handling coverage at 85% exceeds threshold

## Recommendations

1. Pair programming sessions between Alice and Carol
2. Address complexity hotspots in parser module
```
