# Velocity Assessment Metrics

Reference document for metric formulas, thresholds, and interpretation.

---

## Team Velocity Rating

Overall team performance grade for executive reporting.

### Team Productivity Index

**Formula:** `(commits_per_day / team_size) × quality_multiplier`

Where quality_multiplier adjusts for code health:
- Test coverage ≥60%: 1.0
- Test coverage 30-59%: 0.9
- Test coverage <30%: 0.8

### Team Velocity Grades

| Grade | Productivity Index | Interpretation |
|-------|-------------------|----------------|
| A | ≥0.8 commits/dev/day | High-performing team, sustainable pace |
| B | 0.5-0.79 commits/dev/day | Meeting expectations, typical velocity |
| C | 0.3-0.49 commits/dev/day | Below expectations, investigate blockers |
| D | 0.15-0.29 commits/dev/day | Significantly underperforming |
| F | <0.15 commits/dev/day | Critical intervention required |

**Industry Reference:** DORA research indicates elite teams deploy multiple times per day. A healthy team with 4 developers produces 2-4 commits/day (0.5-1.0 per developer).

### Team Health Score

Composite score (0-100) combining:

| Component | Weight | Healthy | Warning | Critical |
|-----------|--------|---------|---------|----------|
| Velocity (commits/dev/day) | 25% | ≥0.5 | 0.3-0.49 | <0.3 |
| Bus Factor | 25% | ≥3 | 2 | 1 |
| Test Coverage | 25% | ≥60% | 30-59% | <30% |
| Error Handling | 25% | ≥80% | 60-79% | <60% |

**Team Health Grades:**

| Grade | Score | Interpretation |
|-------|-------|----------------|
| A | 85-100 | Excellent health, low risk |
| B | 70-84 | Good health, minor improvements needed |
| C | 55-69 | Fair health, multiple areas need attention |
| D | 40-54 | Poor health, significant intervention needed |
| F | <40 | Critical health, immediate action required |

---

## Individual Performance Rating

Per-contributor performance assessment for performance management.

### Expected Contribution Baseline

**Formula:** `expected_share = 100% / active_contributors`

A team of 4 active contributors has an expected share of 25% per person. Performance is measured relative to this baseline.

### Performance Index

**Formula:** `performance_index = (actual_weighted_score / expected_share) × 100`

Where weighted_score combines volume, breadth, consistency, and quality as defined in Contributor Performance Rating section.

### Individual Performance Grades

| Grade | Performance Index | Interpretation |
|-------|------------------|----------------|
| Exceeds Expectations | ≥150% | Significantly above expected contribution |
| Meets Expectations+ | 120-149% | Above expected contribution |
| Meets Expectations | 80-119% | Within expected contribution range |
| Needs Improvement | 50-79% | Below expected contribution |
| Unsatisfactory | <50% | Significantly below expected contribution |

### Performance Grade Calculation Example

**Team:** 4 active contributors
**Expected share:** 25% each

| Contributor | Weighted Score | % of Total | Performance Index | Grade |
|-------------|---------------|------------|-------------------|-------|
| Alice | 35% | 35% | 140% (35/25) | Meets Expectations+ |
| Bob | 30% | 30% | 120% (30/25) | Meets Expectations+ |
| Carol | 25% | 25% | 100% (25/25) | Meets Expectations |
| Dave | 10% | 10% | 40% (10/25) | Unsatisfactory |

### Adjustment Factors

Performance ratings may warrant adjustment based on:

| Factor | Adjustment | Rationale |
|--------|------------|-----------|
| New hire (<90 days) | +1 grade | Ramp-up period expected |
| Part-time allocation | Pro-rate expected share | 50% allocation = 50% of baseline |
| Non-coding role | Exclude from calculation | PM, designer contributions not measured |
| Extended leave | Exclude inactive period | Medical, parental leave |

### Performance Summary Format

For each contributor, generate:

```
Contributor: {name}
Period: {start_date} to {end_date}
Performance Grade: {grade}
Performance Index: {index}% of expected

Key Metrics:
- Commits: {count} ({pct}% of team)
- Lines Changed: {lines} ({pct}% of team)
- Files Touched: {files} ({pct}% of team)
- Active Weeks: {weeks} of {total_weeks} ({consistency}%)

Strengths:
- {strength based on highest dimension score}

Areas for Improvement:
- {area based on lowest dimension score}
```

---

## Velocity Metrics

### Commits Per Day

**Formula:** `total_commits / days_in_period`

| Range | Status | Interpretation |
|-------|--------|----------------|
| >= 2.0 | Healthy | Active development pace |
| 1.0 - 1.9 | Warning | Moderate activity, may indicate blockers |
| < 1.0 | Critical | Low activity, investigate causes |

### Lines Changed Per Commit

**Formula:** `(additions + deletions) / total_commits`

| Range | Status | Interpretation |
|-------|--------|----------------|
| 50 - 200 | Healthy | Well-scoped commits |
| 200 - 500 | Warning | Large commits, review practices needed |
| > 500 | Critical | Monolithic commits, CI/CD risk |
| < 50 | Warning | Very small commits, possible noise |

### Release Frequency

**Formula:** `tags_created / weeks_in_period`

| Range | Status | Interpretation |
|-------|--------|----------------|
| >= 1/week | Healthy | Continuous delivery capability |
| 1/2 weeks | Warning | Bi-weekly releases, acceptable |
| < 1/month | Critical | Infrequent releases, deployment risk |

## Contributor Metrics

### Bus Factor

**Formula:** Minimum contributors covering 50% of commits

| Value | Status | Interpretation |
|-------|--------|----------------|
| >= 3 | Healthy | Knowledge distributed |
| 2 | Warning | Key person dependency emerging |
| 1 | Critical | Single point of failure |

### Gini Coefficient (Contribution Inequality)

**Formula:** Standard Gini calculation on commit distribution

| Range | Status | Interpretation |
|-------|--------|----------------|
| 0.0 - 0.3 | Healthy | Even distribution |
| 0.3 - 0.6 | Warning | Moderate concentration |
| > 0.6 | Critical | Highly concentrated ownership |

### File Ownership

**Formula:** Percentage of files with single dominant contributor (>80% of changes)

| Range | Status | Interpretation |
|-------|--------|----------------|
| < 30% | Healthy | Shared ownership |
| 30% - 60% | Warning | Emerging silos |
| > 60% | Critical | Knowledge silos present |

## Contributor Performance Rating

Multi-dimensional scoring system for individual contributor assessment.

### Dimension Scores

Each contributor receives scores (0-100) across four dimensions:

| Dimension | Weight | Formula | Rationale |
|-----------|--------|---------|-----------|
| Volume | 40% | `commits × log10(lines_changed + 1)` normalized | Rewards impactful work, log dampens LOC gaming |
| Breadth | 20% | `files_touched / total_files_changed × 100` | Measures codebase familiarity |
| Consistency | 20% | `active_weeks / period_weeks × 100` | Rewards sustained engagement |
| Quality | 20% | From quality assessor (default: 50) | Code health contribution |

### Lines Changed vs Net LOC

The system uses **lines changed** (`additions + deletions`) rather than net LOC (`additions - deletions`):

| Metric | Pros | Cons |
|--------|------|------|
| Lines Changed | Values deletions, measures effort | Can reward churn |
| Net LOC | Measures growth | Penalizes cleanup work |

Deletions represent valuable work (removing dead code, refactoring, simplification). Lines changed captures total effort without penalizing contributors who improve code health through removal.

### Weighted Score Calculation

```
weighted_score = (volume × 0.40) + (breadth × 0.20) + (consistency × 0.20) + (quality × 0.20)
```

### Contributor Tiers

Based on percentage of total team weighted score:

| Tier | Score Share | Interpretation |
|------|-------------|----------------|
| Core | ≥25% | Primary contributors, high bus factor impact |
| Regular | 10-24% | Consistent contributors, important for coverage |
| Occasional | 3-9% | Part-time or focused contributors |
| Drive-by | <3% | Minimal involvement, one-off fixes |

### Tier Distribution Health

| Pattern | Status | Interpretation |
|---------|--------|----------------|
| 2+ Core contributors | Healthy | Distributed ownership |
| 1 Core, 2+ Regular | Warning | Emerging concentration |
| 1 Core, few Regular | Critical | Key person dependency |

## Temporal Patterns (Informational Only)

**Not included in risk scoring.** Remote contributors across time zones make commit timestamps unreliable as risk indicators. Report these metrics for visibility only.

### After-Hours Work

**Formula:** `commits_outside_9am_6pm / total_commits * 100`

| Range | Interpretation |
|-------|----------------|
| < 10% | Typical for co-located teams |
| 10% - 25% | Common with some remote contributors |
| > 25% | Likely distributed team or timezone offset |

### Weekend Work

**Formula:** `commits_saturday_sunday / total_commits * 100`

| Range | Interpretation |
|-------|----------------|
| < 5% | Typical for standard work week |
| 5% - 15% | Some weekend activity |
| > 15% | Significant weekend commits |

### Commit Burst Detection

**Pattern:** >10 commits in single hour from one contributor

| Frequency | Interpretation |
|-----------|----------------|
| Rare (< 1/month) | Normal variation |
| Occasional (1-3/month) | Possible deadline pressure or rebasing |
| Frequent (> 3/month) | May indicate squash/rebase workflow |

## Quality Metrics

### Error Handling Coverage

**Formula:** `functions_with_error_handling / total_functions * 100`

| Range | Status | Interpretation |
|-------|--------|----------------|
| > 80% | Healthy | Robust error handling |
| 60% - 80% | Warning | Gaps in error handling |
| < 60% | Critical | Insufficient error handling |

### Complexity Hotspots

**Formula:** Count of functions with cyclomatic complexity > 10

| Count | Status | Interpretation |
|-------|--------|----------------|
| 0 - 5 | Healthy | Well-structured code |
| 6 - 15 | Warning | Some complexity debt |
| > 15 | Critical | Significant refactoring needed |

### Test Coverage Indicators

**Formula:** Ratio of test files to source files

| Ratio | Status | Interpretation |
|-------|--------|----------------|
| > 0.5 | Healthy | Good test coverage |
| 0.3 - 0.5 | Warning | Moderate coverage |
| < 0.3 | Critical | Insufficient testing |

## Per-Contributor Quality Signals (Future)

These metrics require additional git history analysis and are reserved for future implementation:

### Revert Rate

**Formula:** `commits_reverted_by_others / total_commits × 100`

| Range | Interpretation |
|-------|----------------|
| < 2% | Normal, rare mistakes |
| 2-5% | Elevated, review quality concern |
| > 5% | High, process intervention needed |

### Churn Rate

**Formula:** `lines_modified_within_14_days / lines_authored × 100`

Measures how often a contributor's code requires immediate follow-up changes.

| Range | Interpretation |
|-------|----------------|
| < 15% | Stable code |
| 15-30% | Normal iteration |
| > 30% | High churn, design concern |

### Test Association Rate

**Formula:** `commits_touching_test_files / total_commits × 100`

| Range | Interpretation |
|-------|----------------|
| > 40% | Strong testing discipline |
| 20-40% | Moderate testing |
| < 20% | Testing gaps likely |

## DORA Metric Alignment

This assessment aligns with DORA (DevOps Research and Assessment) metrics:

| DORA Metric | Our Proxy |
|-------------|-----------|
| Deployment Frequency | Release frequency |
| Lead Time for Changes | Commits per day (inverse correlation) |
| Change Failure Rate | Not directly measured (requires incident data) |
| Time to Restore | Not directly measured (requires incident data) |

## Risk Score Calculation

**Overall Risk Score:** Weighted average of metrics (higher = more risk)

| Component | Weight | Notes |
|-----------|--------|-------|
| Velocity | 30% | Commit frequency, release cadence |
| Contributor Health | 40% | Bus factor, concentration |
| Code Quality | 30% | Error handling, test coverage |

**Excluded from scoring:** Temporal patterns (after-hours, weekend work) are reported for visibility but not scored. Remote contributors across time zones make commit timestamps unreliable as risk indicators.

**Final Score Interpretation:**

| Score | Status | Action |
|-------|--------|--------|
| 0 - 30 | Low Risk | Continue current practices |
| 31 - 60 | Moderate Risk | Address warning areas |
| 61 - 100 | High Risk | Immediate intervention needed |

## Data Sources

| Metric Category | Data Source | Tool |
|-----------------|-------------|------|
| Velocity | Git log | Bash (git commands) |
| Contributors | Git shortlog, git log --numstat | Bash (git commands) |
| Contributor LOC | Git log --numstat per author | Bash (git commands) |
| Files Touched | Git log --name-only per author | Bash (git commands) |
| Active Weeks | Git log --date=format per author | Bash (git commands) |
| Temporal | Git log timestamps | Bash (git commands) |
| Quality | Symbol analysis | Codanna CLI |
