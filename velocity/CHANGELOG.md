# Changelog

## [1.3.0] - 2026-02-03

### Added

- Team Velocity Grade (A-F) based on commits/developer/day with industry benchmarks
- Team Health Grade (A-F) composite score from velocity, bus factor, test coverage, error handling
- Individual Performance Rating per contributor (Exceeds/Meets+/Meets/Needs Improvement/Unsatisfactory)
- Performance Index calculation: actual contribution % vs expected share (100%/team_size)
- Individual performance cards with dimension breakdown, strengths, and development areas
- Adjustment factor guidance for new hires, part-time allocation, non-coding roles

### Changed

- Executive summary now includes Team Performance Summary table with grades
- Report template includes Individual Performance Ratings section
- Metrics definitions reorganized with Team and Individual sections at top

### Rationale

- Non-technical owners need clear grades for performance management
- Team velocity normalized by team size enables cross-team comparison
- Expected share baseline (100%/n) provides fair individual assessment
- Grade scale aligns with standard HR performance review terminology

## [1.2.0] - 2026-02-03

### Added

- Per-contributor LOC tracking (lines added, lines deleted, lines changed)
- Per-contributor files touched metric for breadth measurement
- Per-contributor active weeks metric for consistency measurement
- Multi-dimensional weighted scoring system (volume 40%, breadth 20%, consistency 20%, quality 20%)
- Contributor tier classification: Core (≥25%), Regular (10-24%), Occasional (3-9%), Drive-by (<3%)
- Dimension scores table showing breakdown per contributor
- Tier distribution summary table
- Future quality signals section (revert rate, churn rate, test association rate)

### Changed

- Contributor breakdown table now shows lines changed, files touched, active weeks, weighted score, and tier
- Replaced Primary/Secondary/Peripheral role classification with tier system based on weighted score share
- Enhanced contributor analysis with tier health assessment and volume/breadth profile analysis
- Updated git analyst with new per-author git commands for LOC and files

### Rationale

- Commits alone fail to capture contribution impact (1-line fix = 1 commit, 5000-line feature = 1 commit)
- Lines changed (additions + deletions) values deletion work (cleanup, refactoring)
- Multi-dimensional scoring prevents gaming any single metric
- Tiers based on weighted score share provide relative benchmarking within teams

## [1.1.0] - 2026-02-03

### Added

- Dashboard benchmark columns showing delta values (over/under percentage)
- Contributor breakdown table with cumulative percentages and role classification
- Contributor analysis section with concentration assessment and risk callouts
- Role classification: Primary (bus factor contributors), Secondary, Peripheral

### Changed

- Dashboard format now includes Benchmark and Delta columns
- Contributor Health section restructured to separate risk indicators from breakdown

## [1.0.0] - 2026-02-03

### Added

- Initial release with velocity assessment skill
- Git analysis agent for commit/contributor patterns
- Quality assessment agent for Codanna-based analysis
- Reporter agent for executive report compilation
- Metrics definitions with DORA alignment
- Report template for consistent output format
- Risk score calculation (0-100 scale)
- Bus factor and Gini coefficient calculations
- Temporal pattern analysis (informational only)
