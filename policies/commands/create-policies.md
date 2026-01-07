---
description: Create policies by extracting patterns from existing artifacts
argument-hint: <policy-prefix> [spec-or-pattern]
---

# Create Policies

Extract patterns from existing code to create policy documents. Policies codify conventions that already exist in the codebase, ensuring agents follow established patterns.

## Step 1: Validate Input

| Argument | Required | Description |
| -------- | -------- | ----------- |
| $1 | Yes | Policy prefix (e.g., API, AUTH, TEST) — 2-6 uppercase chars |
| $2 | No | Spec path (`.md`) OR glob pattern (e.g., `src/**/*.py`) |

If $2 omitted, prompt user for file pattern.

## Step 2: Identify Artifact Files

**If spec provided ($2 ends in `.md`):**
```
Read artifact spec at $2
Extract file patterns from registry entry
```

**If glob pattern provided:**
```
Use $2 directly as glob pattern
```

**Find matching files:**
```
Glob for files matching pattern(s)
Report: Found N files matching [patterns]
```

## Step 3: Extract Patterns

```
Task(@Explore, "Extract policy rules from existing artifacts:

Files to analyze: [matched files from Step 2]

For each file, identify:
1. Naming conventions — file names, function/class names, variables
2. Structure patterns — imports, organization, module layout
3. Code idioms — common patterns, error handling, logging
4. Framework usage — how frameworks are used
5. Documentation style — docstrings, comments, type hints

Aggregate findings:

| Pattern | Frequency | Confidence |
| ------- | --------- | ---------- |
| [pattern] | X% of files | High/Medium/Low |

Output: Patterns report with frequency table.")
```

## Step 4: Check Existing Policies

Read `.claude/policies/` directory:
- Find existing policies with similar scope
- Identify reusable content vs new content needed
- Note overlaps to avoid duplication

## Step 5: Create Policy

```
Task(@policy-engineer, "Create policy §$1 from extracted patterns.

Location: .claude/policies/policies-$1.md

Input:
- Extracted patterns: [from Step 3]
- Files analyzed: [N files]
- Existing policies: [from Step 4]

Structure:
- {§$1.1} File Organization — naming/structure rules
- {§$1.2} Code Patterns — idioms with correct/incorrect examples
- {§$1.3} Framework Usage — framework conventions
- {§$1.4} Prohibited Patterns — anti-patterns to avoid

For each rule:
- State context (WHY this rule exists)
- State the rule explicitly (no vague language)
- Show correct AND incorrect examples (under 10 lines each)
- Note frequency (e.g., 'found in 90% of files')

Follow §META limits: <500 lines total, <50 lines per section, <10 lines per example.")
```

## Step 6: Review Policy

```
Task(@policy-reviewer, "Review .claude/policies/policies-$1.md against §META.

Verify:
- All rules pass actionability test (no vague language)
- Each rule has context explaining WHY
- Examples show correct AND incorrect
- Frequency/confidence noted for each rule
- Under 500 lines total")
```

## Step 7: Report

| Item | Value |
| ---- | ----- |
| Policy created | `.claude/policies/policies-$1.md` |
| Source files | [N files matching pattern] |
| Sections | [count] |
| Review verdict | Pass/Fail |

## Important Rules

| Rule | Rationale |
| ---- | --------- |
| Extract from actual files | Policies must reflect real patterns, not assumptions |
| Include frequency data | Confidence comes from prevalence in codebase |
| Show correct AND incorrect | Contrast clarifies boundaries |
| Examples under 10 lines | Longer examples dilute impact |
| Policy under 500 lines | Long policies reduce compliance |
