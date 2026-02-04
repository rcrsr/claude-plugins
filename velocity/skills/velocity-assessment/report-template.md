# Velocity Report Template

Template for executive velocity assessment reports.

## Report Structure

```markdown
# Development Velocity Report

**Period:** {start_date} to {end_date} ({days} days)
**Branch:** {branch}
**Generated:** {timestamp}

---

## Executive Summary

{2-3 sentence overview of velocity health and key findings}

### Dashboard

| Metric | Value | Benchmark | Delta | Status |
|--------|-------|-----------|-------|--------|
| Commits/Day | {value} | 2.0 | {delta} ({pct}% {over/under}) | {status} |
| Bus Factor | {value} | 3 | {delta} ({pct}% {over/under}) | {status} |
| Code Quality | {value}% | 80% | {delta}pp ({pct}% {over/under}) | {status} |

**Overall Risk Score:** {score}/100 ({risk_level})

---

## Velocity Analysis

### Commit Activity

| Metric | Value | Threshold | Status |
|--------|-------|-----------|--------|
| Total Commits | {value} | - | - |
| Commits/Day | {value} | >=2.0 | {status} |
| Lines/Commit | {value} | 50-200 | {status} |
| Release Count | {value} | - | - |

### Activity Trend

{Weekly commit counts or visual representation}

---

## Contributor Health

### Risk Indicators

| Metric | Value | Threshold | Status |
|--------|-------|-----------|--------|
| Bus Factor | {value} | >=3 | {status} |
| Gini Coefficient | {value} | <0.3 | {status} |
| File Ownership | {value}% | <30% | {status} |

### Contributor Breakdown

| Contributor | Commits | % of Total | Cumulative % | Role |
|-------------|---------|------------|--------------|------|
| {name} | {count} | {pct}% | {cumulative}% | {Primary/Secondary/Peripheral} |
| ... | ... | ... | ... | ... |

### Contributor Analysis

{Analysis covering:}
- Concentration assessment with specific percentages
- Knowledge distribution and silo identification
- Comparison to ideal distribution (100/n% per contributor)
- Specific risk callouts (single point of failure, burnout indicators)

---

## Temporal Patterns

### Work Distribution

| Pattern | Value | Threshold | Status |
|---------|-------|-----------|--------|
| After-Hours | {value}% | <10% | {status} |
| Weekend Work | {value}% | <5% | {status} |
| Burst Commits | {count}/month | <1/month | {status} |

### Hour Distribution

{Hourly commit distribution summary}

---

## Code Quality Assessment

### Quality Indicators

| Metric | Value | Threshold | Status |
|--------|-------|-----------|--------|
| Error Handling | {value}% | >80% | {status} |
| Complexity Hotspots | {count} | <5 | {status} |
| Test Ratio | {value} | >0.5 | {status} |

### Hotspot Areas

{List of files/modules with quality concerns}

---

## Key Findings

1. {Finding with supporting data}
2. {Finding with supporting data}
3. {Finding with supporting data}

---

## Recommendations

### Immediate Actions (This Sprint)

1. {Actionable recommendation}
2. {Actionable recommendation}

### Short-Term (1-2 Sprints)

1. {Actionable recommendation}
2. {Actionable recommendation}

### Long-Term (Quarter)

1. {Strategic recommendation}
2. {Strategic recommendation}

---

## Appendix: Raw Data

<details>
<summary>Git Analysis JSON</summary>

\`\`\`json
{git_analysis_json}
\`\`\`

</details>

<details>
<summary>Quality Assessment JSON</summary>

\`\`\`json
{quality_assessment_json}
\`\`\`

</details>
```

## Status Indicators

| Symbol | Meaning | CSS Class |
|--------|---------|-----------|
| Healthy | Healthy | `.status-healthy` |
| Warning | Warning | `.status-warning` |
| Critical | Critical | `.status-critical` |

## Trend Indicators

| Symbol | Meaning |
|--------|---------|
| up | Improving (vs previous period) |
| down | Declining (vs previous period) |
| stable | Stable (within 5% variance) |

## Risk Level Criteria

| Level | Score Range | Description |
|-------|-------------|-------------|
| Low | 0-30 | Sustainable pace, no immediate concerns |
| Moderate | 31-60 | Areas need attention, not urgent |
| High | 61-100 | Significant issues requiring intervention |

## Writing Guidelines

### Executive Summary

- Lead with the most critical finding
- Use specific numbers, not vague descriptors
- 3 sentences maximum
- Include the risk score prominently

### Findings Section

Each finding must include:
- Specific metric value
- Comparison to threshold
- Business impact statement

**Good:** "Bus factor of 2 indicates key-person risk. Two contributors account for 85% of commits. Team expansion or knowledge transfer recommended."

**Bad:** "The team is somewhat dependent on a few people."

### Recommendations Section

Each recommendation must be:
- Specific and actionable
- Tied to a finding
- Assigned a timeframe
- Measurable

**Good:** "Pair programming for parser module with secondary contributor by sprint 3."

**Bad:** "Improve code review practices."

## Data Formatting

### Numbers

- Percentages: One decimal place (e.g., 78.5%)
- Ratios: Two decimal places (e.g., 0.42)
- Counts: No decimals (e.g., 127 commits)
- Dates: ISO format (e.g., 2026-02-03)

### Tables

- Right-align numbers
- Left-align text
- Center status indicators
- Include units in headers

### JSON Output

- Pretty-print with 2-space indentation
- Wrap in code fences
- Collapse in details element for length
