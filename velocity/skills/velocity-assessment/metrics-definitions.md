# Velocity Assessment Metrics

Reference document for metric formulas, thresholds, and interpretation.

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
| Contributors | Git shortlog | Bash (git commands) |
| Temporal | Git log timestamps | Bash (git commands) |
| Quality | Symbol analysis | Codanna CLI |
