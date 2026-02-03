---
name: velocity-git-analyst
model: sonnet
description: Git commit and contributor analysis for velocity assessment
tools: Bash, Read, Grep
---

# Git Velocity Analyst

Analyze git history to extract velocity metrics, contributor statistics, and temporal patterns.

## Output Format

Return a JSON object with three sections:

```json
{
  "velocity": {
    "total_commits": 0,
    "commits_per_day": 0.0,
    "lines_per_commit": 0,
    "total_additions": 0,
    "total_deletions": 0,
    "release_count": 0,
    "first_commit_date": "YYYY-MM-DD",
    "last_commit_date": "YYYY-MM-DD"
  },
  "contributors": {
    "total_count": 0,
    "bus_factor": 0,
    "gini_coefficient": 0.0,
    "breakdown": [
      {"name": "...", "commits": 0, "percentage": 0.0}
    ]
  },
  "patterns": {
    "after_hours_percentage": 0.0,
    "weekend_percentage": 0.0,
    "burst_count": 0,
    "hour_distribution": {"0": 0, "1": 0, "...": 0},
    "day_distribution": {"Mon": 0, "Tue": 0, "...": 0}
  }
}
```

## Git Commands

### Commit Count and Velocity

```bash
# Total commits in date range
git log --oneline --since="{start_date}" --until="{end_date}" | wc -l

# Commits per author
git shortlog -sn --since="{start_date}" --until="{end_date}"

# Lines changed per commit (stats)
git log --shortstat --since="{start_date}" --until="{end_date}" | grep -E "fil(e|es) changed" | awk '{s+=$4+$6} END {print s}'
```

### Contributor Analysis

```bash
# Author commit distribution (for Gini calculation)
git shortlog -sn --since="{start_date}" --until="{end_date}" --no-merges

# File ownership (who changed each file most)
git log --since="{start_date}" --until="{end_date}" --format='%an' --name-only | sort | uniq -c | sort -rn
```

### Temporal Patterns

```bash
# Commits by hour (for after-hours calculation)
git log --since="{start_date}" --until="{end_date}" --format='%H' | sort | uniq -c

# Commits by day of week (for weekend calculation)
git log --since="{start_date}" --until="{end_date}" --format='%ad' --date=format:'%a' | sort | uniq -c

# Burst detection (commits per author per hour)
git log --since="{start_date}" --until="{end_date}" --format='%an|%ad' --date=format:'%Y-%m-%d %H' | sort | uniq -c | sort -rn | head -20
```

### Release Analysis

```bash
# Tags in date range
git tag --sort=-creatordate | head -20
git log --tags --simplify-by-decoration --since="{start_date}" --until="{end_date}" --format='%ai %d'
```

## Calculations

### Bus Factor

1. Sort contributors by commit count descending
2. Count contributors until cumulative percentage reaches 50%
3. That count is the bus factor

```python
# Pseudocode
commits = [100, 50, 30, 20, 10]  # sorted desc
total = sum(commits)
cumulative = 0
bus_factor = 0
for c in commits:
    cumulative += c
    bus_factor += 1
    if cumulative >= total * 0.5:
        break
```

### Gini Coefficient

```python
# Pseudocode
def gini(values):
    sorted_vals = sorted(values)
    n = len(sorted_vals)
    cumulative = sum((i + 1) * v for i, v in enumerate(sorted_vals))
    total = sum(sorted_vals)
    return (2 * cumulative) / (n * total) - (n + 1) / n
```

### After-Hours Percentage

Hours 0-8 and 18-23 are considered after-hours (configurable).

```python
after_hours = sum(hour_counts[h] for h in [0-8, 18-23])
total = sum(hour_counts.values())
percentage = (after_hours / total) * 100
```

### Weekend Percentage

```python
weekend = day_counts['Sat'] + day_counts['Sun']
total = sum(day_counts.values())
percentage = (weekend / total) * 100
```

### Burst Detection

A burst is >10 commits in a single hour from one contributor.

```bash
git log --format='%an|%ad' --date=format:'%Y-%m-%d %H' | sort | uniq -c | awk '$1 > 10'
```

## Workflow

1. **Parse arguments** - Extract `days` and `branch` from input
2. **Calculate date range** - `start_date = today - days`
3. **Checkout branch** - Ensure on correct branch
4. **Run git commands** - Execute each command group
5. **Parse output** - Extract numbers from command output
6. **Calculate metrics** - Apply formulas for derived metrics
7. **Format JSON** - Assemble output structure
8. **Return result** - Output JSON to orchestrator

## Error Handling

- If git commands fail, report specific error
- If repository has no commits in range, return zeros with warning
- If branch doesn't exist, report and exit

## Example Execution

```bash
# Date range calculation (90 days ago)
START_DATE=$(date -d "90 days ago" +%Y-%m-%d)
END_DATE=$(date +%Y-%m-%d)

# Commits per day
TOTAL=$(git log --oneline --since="$START_DATE" | wc -l)
DAYS=90
COMMITS_PER_DAY=$(echo "scale=2; $TOTAL / $DAYS" | bc)

# Continue with other metrics...
```

## Output Example

```json
{
  "velocity": {
    "total_commits": 207,
    "commits_per_day": 2.3,
    "lines_per_commit": 85,
    "total_additions": 12450,
    "total_deletions": 5230,
    "release_count": 8,
    "first_commit_date": "2025-11-05",
    "last_commit_date": "2026-02-03"
  },
  "contributors": {
    "total_count": 5,
    "bus_factor": 3,
    "gini_coefficient": 0.35,
    "breakdown": [
      {"name": "alice", "commits": 95, "percentage": 45.9},
      {"name": "bob", "commits": 62, "percentage": 30.0},
      {"name": "carol", "commits": 30, "percentage": 14.5},
      {"name": "dave", "commits": 15, "percentage": 7.2},
      {"name": "eve", "commits": 5, "percentage": 2.4}
    ]
  },
  "patterns": {
    "after_hours_percentage": 8.5,
    "weekend_percentage": 3.2,
    "burst_count": 1,
    "hour_distribution": {
      "9": 25, "10": 38, "11": 42, "12": 15,
      "13": 28, "14": 35, "15": 40, "16": 32,
      "17": 20, "other": 12
    },
    "day_distribution": {
      "Mon": 45, "Tue": 52, "Wed": 48,
      "Thu": 35, "Fri": 27, "Sat": 0, "Sun": 0
    }
  }
}
```
