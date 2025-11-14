---
description: Initialize CHANGELOG.md and RELEASE_NOTES.md for a new project or retroactively from git history
aliases: ["cl-init"]
---

# changelog-init

Initialize changelog documentation for a project by analyzing entire git history
and generating both developer and user-facing documentation files.

## Usage

```bash
/changelog-init                    # Interactive initialization
/changelog-init --from-beginning  # Analyze entire git history
/changelog-init --from-tag v1.0.0 # Start from specific version
/changelog-init --empty           # Create empty template files

# Historical Replay Mode (NEW)
/changelog-init --replay           # Interactive period-based replay
/changelog-init --replay --period monthly    # Monthly period grouping
/changelog-init --replay --period weekly     # Weekly period grouping
/changelog-init --replay --period by-release # Group by releases
/changelog-init --replay --period auto       # Auto-detect best period
```

## Workflow

### Step 1: Project Analysis

- Detects existing version files (package.json, setup.py, etc.)
- Identifies versioning scheme in use
- Analyzes git tag patterns
- Detects commit message conventions

### Step 2: History Processing

- Extracts all commits from starting point
- Groups by releases (if tags exist)
- Categorizes changes
- Identifies key milestones

### Step 3: File Generation

- Creates CHANGELOG.md with complete history
- Creates RELEASE_NOTES.md with major highlights
- Generates .changelog.yaml configuration
- Sets up .changelog-metadata.json for tracking

## Replay Workflow (Historical Period-Based Generation)

The `--replay` mode generates changelogs by "replaying" history through time periods (daily/weekly/monthly/by-release), creating changelog entries as if they were updated in real-time during development.

### Phase 1: Period Detection

**Agent: period-detector (Claude Haiku)**

```
🔍 Analyzing repository structure...

Repository: user/awesome-project
First commit: 2023-03-15 (619 days ago)
Total commits: 523
Contributors: 24
Git tags: 12 releases detected

Auto-detecting optimal period strategy...
✅ Recommended: weekly (21.4 commits/week average)

Period options:
1. Weekly (74 periods, ~7 commits/period)
2. Monthly (20 periods, ~26 commits/period)
3. By-Release (12 periods, ~44 commits/period)
4. Auto (use recommended: weekly)

Select period strategy [1-4] or press Enter for auto: _
```

**Detection Logic:**
- Analyzes commit frequency and distribution
- Detects existing release tags
- Calculates project age and activity patterns
- Provides smart recommendations with rationale

### Phase 2: Boundary Calculation

**Agent: period-detector (Claude Haiku)**

```
📅 Calculating period boundaries...

Strategy: Weekly (Monday-Sunday)
Total periods: 74
Empty periods (will be skipped): 8
Merge-only periods (will be skipped): 3
Active periods to analyze: 63

Period boundaries:
- Week 1:  2023-03-13 to 2023-03-19 (15 commits)
- Week 2:  2023-03-20 to 2023-03-26 (22 commits)
- Week 3:  2023-03-27 to 2023-04-02 (18 commits)
...
- Week 74: 2025-11-10 to 2025-11-16 (8 commits) [Unreleased]

First period note: Week 1 has 287 commits (initial development)
→ Will use summary mode for clarity

Continue with analysis? [Y/n]: _
```

**Boundary Features:**
- Calendar-aligned periods (weeks start Monday, months are calendar months)
- Automatic skip of empty periods
- Automatic skip of merge-only periods
- First-period summarization for large initial development
- Partial final period handling (→ [Unreleased])

### Phase 3: Parallel Period Analysis

**Agent: period-coordinator (Claude Sonnet)**

```
⚙️ Orchestrating multi-period analysis...

Execution plan:
- Total periods: 63
- Concurrent workers: 3
- Estimated time: ~4 minutes
- Cache enabled: Yes

Batch 1/21: Analyzing periods 1-3 in parallel...
 ✅ Week 1 (2023-03-13): 287 commits → Summarized
 ✅ Week 2 (2023-03-20): 22 commits → 18 changes categorized
 ✅ Week 3 (2023-03-27): 18 commits → 15 changes categorized

Batch 2/21: Analyzing periods 4-6 in parallel...
 ✅ Week 4 (2023-04-03): 25 commits → 20 changes categorized
 ⚠️ Week 5 (2023-04-10): Analysis timeout, retrying...
 ✅ Week 5 (2023-04-10): 31 commits → 28 changes categorized
 ✅ Week 6 (2023-04-17): 19 commits → 16 changes categorized

[Progress: 9/63 periods complete - 14% - ETA 3m 42s]
...
```

**Per-Period Analysis:**
Each period invokes:
1. **git-history-analyzer** (Sonnet): Extract and group commits for period
2. **commit-analyst** (Haiku): Batch-analyze unclear commits
3. Caching: Results cached per period for instant regeneration

### Phase 4: Changelog Synthesis

**Agent: changelog-synthesizer (Claude Sonnet)**

```
📝 Generating hybrid-format changelog...

Format: Hybrid (version releases with period subsections)
Structure:
- [Unreleased] section: 2 active periods
- Released versions: 12 versions across 61 periods
- Total entries: 412 changes categorized

Generating CHANGELOG.md...
✅ 63 period sections created
✅ 12 version sections created
✅ Navigation TOC added (74 periods)
✅ 412 changes formatted

Generating RELEASE_NOTES.md...
✅ Period-aware user-facing notes created
✅ Adapted tone for end-users
✅ Highlighted major features per period
```

**Output Format:**
- **CHANGELOG.md**: Hybrid format with version sections containing period subsections
- **RELEASE_NOTES.md**: User-friendly with period-based highlights
- **Navigation**: Table of contents for changelogs >10 periods

### Phase 5: Verification and Commit

```
✅ Changelog generation complete!

Files created:
- CHANGELOG.md (1,847 lines, 63 period sections)
- RELEASE_NOTES.md (423 lines, user-friendly format)
- .changelog.yaml (configuration persisted)
- .changelog-cache/ (63 period analyses cached)

Preview of CHANGELOG.md:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Changelog

## [Unreleased]

### Week of November 11, 2025
#### Added
- Real-time notification system (#256)

### Week of November 4, 2025
#### Added
- Advanced search filters (#252)

## [2.1.0] - 2024-10-27

### Week of October 21, 2024
#### Added
- REST API v2 with pagination (#234)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Actions:
1. Review generated files? [Y/n]
2. Commit to git? [Y/n]
3. Create version tag? [y/N]
```

## Options

### Standard Mode

- `--from-beginning`: Analyze entire repository history
- `--from-tag <tag>`: Start from specific git tag
- `--empty`: Create template files without historical content
- `--config-only`: Only generate .changelog.yaml configuration
- `--force`: Overwrite existing files (backup created)

### Replay Mode (NEW)

- `--replay`: Enable historical replay mode with period-based generation
- `--period <strategy>`: Period grouping strategy
  - `daily`: One entry per day with commits
  - `weekly`: One entry per week (Monday-Sunday)
  - `monthly`: One entry per calendar month
  - `quarterly`: One entry per quarter (Q1, Q2, Q3, Q4)
  - `annual`: One entry per year
  - `by-release`: One entry per git tag/release
  - `auto`: Auto-detect best strategy (default)
- `--calendar-align`: Align periods to calendar boundaries (default: true)
- `--skip-empty`: Skip periods with no commits (default: true)
- `--skip-merge-only`: Skip periods with only merge commits (default: true)
- `--max-workers <n>`: Concurrent period analyses (default: 3, max: 5)
- `--no-cache`: Disable caching (slower but always fresh)
- `--hybrid-format`: Generate hybrid format CHANGELOG.md (default: true)
- `--replay-in-release-notes`: Apply replay structure to RELEASE_NOTES.md (default: true)

## Examples

### New Project Setup

```bash
You: /changelog-init --empty

Claude: 📝 Initializing changelog files...

Created:
✅ CHANGELOG.md (template)
✅ RELEASE_NOTES.md (template)
✅ .changelog.yaml (configuration)

Your changelog files are ready! Next steps:
1. Customize .changelog.yaml for your project
2. Run `/changelog update` after making changes
3. Run `/changelog release` when ready to publish
```

### Retroactive Generation

```bash
You: /changelog-init --from-beginning

Claude: 📊 Analyzing repository history...

Found:
- 523 commits
- 12 tagged releases
- 24 contributors
- First commit: 2023-03-15

Generating comprehensive changelog...

✅ Generated CHANGELOG.md with 12 versions
✅ Generated RELEASE_NOTES.md with major milestones
✅ Created configuration files

Notable milestones detected:
- v1.0.0: Initial release
- v2.0.0: Major rewrite with breaking changes
- v2.5.0: Performance optimizations
```

### Replay Mode: Auto-Detection

```bash
You: /changelog-init --replay

Claude: 🔍 Analyzing repository for replay...

Repository analysis:
- Total commits: 523
- First commit: 2023-03-15 (619 days ago)
- Releases: 12 tagged versions
- Contributors: 24
- Commit frequency: 21.4 commits/week

Auto-detection results:
✅ Recommended strategy: weekly
   Rationale: Consistent activity (15-30 commits/week)

Alternative strategies:
- Monthly: 20 periods, ~26 commits/period
- By-Release: 12 periods, ~44 commits/period

Use recommended strategy? [Y/n]: Y

📅 Calculating 74 weekly periods...
- Active periods: 63
- Empty periods (skipped): 8
- Merge-only periods (skipped): 3

Proceed with analysis? [Y/n]: Y

⚙️ Executing parallel analysis (3 workers)...
[Progress bar] 63/63 periods analyzed - 100% - 3m 42s elapsed

✅ Generated CHANGELOG.md (1,847 lines, hybrid format)
✅ Generated RELEASE_NOTES.md (423 lines, period-aware)
✅ Configuration saved to .changelog.yaml
✅ Cache created: .changelog-cache/ (63 period analyses)
```

### Replay Mode: Monthly Periods

```bash
You: /changelog-init --replay --period monthly

Claude: 📅 Generating changelog with monthly periods...

Period breakdown:
- March 2023: 45 commits
- April 2023: 67 commits
- May 2023: 52 commits
...
- November 2025: 23 commits (partial, → [Unreleased])

Total: 20 active months, 3 empty months (skipped)

Analyzing in parallel (3 workers)...
✅ Complete! Generated hybrid CHANGELOG.md with 20 month sections.
```

### Replay Mode: Release-Based

```bash
You: /changelog-init --replay --period by-release

Claude: 🏷️ Grouping history by releases...

Detected releases:
- v0.1.0 (2023-03-15): 15 commits
- v0.2.0 (2023-04-12): 28 commits
- v1.0.0 (2023-06-01): 67 commits (major release)
- v1.1.0 (2023-07-15): 34 commits
...
- v2.5.0 (2024-10-27): 42 commits
- Unreleased: 23 commits

Total: 12 releases + unreleased section

Analyzing releases...
✅ Generated CHANGELOG.md with 13 sections (12 releases + unreleased)
```

### Replay Mode: High-Activity Project (Daily)

```bash
You: /changelog-init --replay --period daily

Claude: ⚠️ Daily period strategy selected

Repository has 523 commits over 619 days
This will create ~619 daily periods

Recommendation: Weekly or monthly might be more readable
Continue with daily strategy? [y/N]: y

📅 Calculating daily periods...
- Active days: 287
- Empty days (skipped): 332

Proceed? This may take ~8 minutes [Y/n]: Y

⚙️ Analyzing 287 daily periods (3 workers)...
[Progress: 287/287 - 100% - 7m 53s]

✅ Generated CHANGELOG.md (3,421 lines)
⚠️ Note: Changelog is very large. Consider using weekly strategy.
```

### Replay Mode: Custom Configuration

```bash
You: /changelog-init --replay --period weekly --max-workers 5 --hybrid-format

Claude: 📋 Replay configuration:
- Period: Weekly (Monday-Sunday)
- Workers: 5 concurrent analyses
- Format: Hybrid (versions + periods)
- Cache: Enabled
- Skip empty: Yes
- Skip merge-only: Yes

⚙️ Executing with 5 concurrent workers...
[Faster execution with more parallelism]

✅ Complete in 2m 18s (vs estimated 4m with 3 workers)
```

### Replay Mode: Regenerate Specific Period

```bash
You: /changelog-init --replay-period "2024-Q3" --force

Claude: 🔄 Regenerating specific period...

Period: Q3 2024 (July-September)
- Original analysis: 2025-01-15
- Commits in period: 87
- Previous entries: 64 changes

Regenerating with current configuration...
✅ Period regenerated
✅ Updated CHANGELOG.md (merged new analysis)
✅ Backup created: CHANGELOG.2025-11-14.md.backup
```

## Generated Files

### CHANGELOG.md Template

```markdown
# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial project setup

### Changed

### Deprecated

### Removed

### Fixed

### Security
```

### Configuration File

```yaml
# .changelog.yaml
changelog:
  conventional_commits: true
  include_authors: true
  include_commit_hash: false
  categories:
    - Added
    - Changed
    - Deprecated
    - Removed
    - Fixed
    - Security

release_notes:
  audience: "end-users"
  tone: "professional"
  use_emoji: true

versioning:
  strategy: "semver"
  auto_bump:
    breaking: "major"
    features: "minor"
    fixes: "patch"

replay:
  enabled: false
  default_strategy: "auto"
  performance:
    enable_parallel: true
    max_concurrent_periods: 3
    enable_cache: true
```

### Hybrid Format CHANGELOG.md (Replay Mode)

```markdown
# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## Table of Contents

- **2025**
  - [Week of November 11](#week-of-november-11-2025) - Unreleased
  - [Week of November 4](#week-of-november-4-2025) - Unreleased
  - [Week of October 21](#week-of-october-21-2024) - v2.1.0
  - [Week of October 14](#week-of-october-14-2024) - v2.1.0
- **2024**
  - [Month of September](#month-of-september-2024) - v2.0.0
  ...

## [Unreleased]

### Week of November 11, 2025

#### Added
- Real-time notification system with WebSocket support (#256, @dev2)
  - Automatic reconnection with exponential backoff
  - Event types: entity.created, entity.updated, entity.deleted

#### Fixed
- Memory leak in background job processor (#258, @dev1)

### Week of November 4, 2025

#### Added
- Advanced search filters with fuzzy matching (#252, @dev3)

## [2.1.0] - 2024-10-27

### Week of October 21, 2024

#### Added
- REST API v2 with cursor-based pagination (#234, @dev1)
  - Backward compatible with v1 using version headers
  - See migration guide in docs/api/v2-migration.md

#### Changed
- **BREAKING:** Authentication now uses JWT tokens instead of server sessions
  - Sessions will be invalidated upon upgrade
  - Update client libraries to v2.x for compatibility

### Week of October 14, 2024

#### Fixed
- Race condition in concurrent file uploads (#245, @dev2)
  - Implemented proper file locking mechanism

#### Security
- Updated dependencies to address CVE-2024-1234 (High severity)

## [2.0.0] - 2024-09-30

### Month of September 2024

#### Added
- Complete UI redesign with modern component library (#210, @design-team)
- Dark mode support across all views (#215, @dev4)

#### Changed
- Migrated from Webpack to Vite
  - Development server startup reduced from 30s to 3s
  - Bundle size reduced by 22%

#### Removed
- Legacy XML export format (use JSON or CSV)
- Python 3.7 support (minimum version now 3.8)

[Unreleased]: https://github.com/user/repo/compare/v2.1.0...HEAD
[2.1.0]: https://github.com/user/repo/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/user/repo/releases/tag/v2.0.0
```

## Integration

This command works with:

- `/changelog update` - Add new changes
- `/changelog release` - Prepare releases
- Git hooks - Automate updates
