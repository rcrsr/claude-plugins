---
description: Review all policy files against META standards in parallel
---

# Review All Policies

Audit all policy documents in `.claude/policies/` against §META standards. Non-compliant policies waste tokens and reduce agent compliance.

## Step 1: Enumerate Policy Files

```bash
ls -1 .claude/policies/*.md
```

## Step 2: Parallel Review

For EACH policy file, spawn a `policy-reviewer` in parallel:

```
Task(@policy-reviewer, "Review .claude/policies/{filename} against §META.

Check:
- Structure: title, TOC, {§PREFIX.X} headers, {§END} marker
- Size: <500 lines doc, <50 lines section, <10 lines example
- Content: explicit rules, context before rules, correct/incorrect pairs, tables
- References: all §PREFIX.X resolve to valid sections

Generate report with metrics, findings by severity, recommendations.")
```

Launch ALL reviews simultaneously using parallel Task calls.

## Step 3: Aggregate Results

After all reviews complete, produce summary:

```markdown
# Policy Review Summary

## Overview

| Policy | Lines | Critical | High | Medium | Status |
| ------ | ----- | -------- | ---- | ------ | ------ |
| §PREFIX | X | 0 | 0 | 2 | Pass |
| §OTHER | Y | 1 | 0 | 0 | Fail |

## Critical Issues

[List Critical findings — must fix before use]

## High Priority

[List High findings — fix soon]

## Medium Priority

[List Medium findings — fix when convenient]

## Next Steps

1. [Specific action with file:line reference]
2. [Specific action with file:line reference]
```

## Pass/Fail Criteria

| Verdict | Condition |
| ------- | --------- |
| Pass | Zero Critical AND zero High violations |
| Fail | Any Critical OR any High violation |

## Execution Notes

| Condition | Action |
| --------- | ------ |
| More than 5 policy files | Use `run_in_background: true` for each Task |
| Background tasks used | Wait for all to complete before aggregating |
| Mixed results | List failing policies first in summary |
