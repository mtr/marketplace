# Changelog Manager Plugin for Claude Code

Intelligent changelog and release notes management through AI-powered git
history analysis. This plugin automates the creation and maintenance of both
technical changelogs (`CHANGELOG.md`) and user-facing release notes
(`RELEASE_NOTES.md`) by analyzing your git history and understanding code
changes at a semantic level.

## Features

- 🤖 **AI-Powered Commit Analysis**: Uses Claude 4.5 Sonnet to understand vague commit
  messages and complex code changes
- 🔗 **GitHub Integration**: Automatically matches commits to Issues, PRs, Projects, and Milestones
  using intelligent multi-strategy matching with confidence scoring
- 📚 **Dual Documentation**: Generates both developer-focused changelogs and
  user-friendly release notes
- 🔍 **Intelligent Grouping**: Automatically groups related commits by PR,
  feature branch, or semantic similarity
- ⚡ **Efficient Processing**: Optimized for large repositories with extensive
  commit history, with smart caching for GitHub data
- 📋 **Standards Compliance**:
  Follows [Keep a Changelog](https://keepachangelog.com)
  and [Semantic Versioning](https://semver.org)
- 🔄 **Incremental Updates**: Only processes new commits since last update
- 🎯 **Breaking Change Detection**: Automatically identifies and documents
  breaking changes
- 🌍 **Multi-language Support**: Understands commits and code in multiple
  programming languages
- ⏱️ **Historical Replay Mode** (NEW): Generate changelogs with period-based granularity (daily/weekly/monthly/by-release)
  as if they were updated in real-time throughout development history

## Installation

### Via Claude Code CLI

```bash
# Add the marketplace (if not already added)
/plugin marketplace add mtr/marketplace/changelog-manager

# Install the plugin
/plugin install changelog-manager

# Verify installation
/help changelog
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/mtr/marketplace/changelog-manager.git ~/.claude/plugins/changelog-manager

# Restart Claude Code
```

## Quick Start

### Initialize Changelog for New Project

```bash
# Create empty changelog templates
/changelog-init --empty

# Or analyze entire git history
/changelog-init --from-beginning

# Or use historical replay mode (generates period-based changelog)
/changelog-init --replay
```

### Update Existing Changelogs

```bash
# Interactive update
/changelog

# Quick update with recent changes
/changelog update
```

### Prepare a Release

```bash
# Interactive release wizard
/changelog-release

# Specific version bump
/changelog-release minor
```

## Commands

### `/changelog`

Main command for interactive changelog management. Guides you through analyzing
commits, categorizing changes, and generating documentation.

**Usage:**

```bash
/changelog # Interactive mode
/changelog update # Update with recent changes
/changelog review # Review and refine existing entries
```

### `/changelog-init`

Initialize changelog files for a new project or generate from existing git
history.

**Usage:**

```bash
/changelog-init --empty # Create templates
/changelog-init --from-beginning # Analyze all history
/changelog-init --from-tag v1.0.0 # Start from specific version

# Historical Replay Mode (NEW)
/changelog-init --replay # Interactive period-based replay
/changelog-init --replay --period weekly # Weekly periods
/changelog-init --replay --period monthly # Monthly periods
/changelog-init --replay --period by-release # Group by releases
/changelog-init --replay --period auto # Auto-detect best strategy
```

**Replay Mode**: Generates changelogs by "replaying" history through time periods
(daily/weekly/monthly/by-release), creating entries as if updated in real-time. Perfect
for retroactive changelog generation with period-based organization.

### `/changelog-release`

Prepare a new release with version bumping, changelog finalization, and release
artifacts.

**Usage:**

```bash
/changelog-release # Interactive wizard
/changelog-release patch # Patch version bump
/changelog-release v2.5.0 # Specific version
```

## Agents

The plugin employs specialized AI agents for different capabilities:

### Core Agents

#### Git History Analyzer

- **Model**: Claude 4.5 Sonnet
- **Purpose**: Analyzes commit history, groups related changes, detects patterns
- **Capabilities**: Branch analysis, PR correlation, semantic clustering,
  version detection, period-scoped extraction

#### Commit Analyst

- **Model**: Claude 4.5 Haiku (optimized for efficiency)
- **Purpose**: Deep analysis of individual commits and code diffs
- **Capabilities**: Diff interpretation, impact assessment, purpose extraction,
  breaking change detection, batch period analysis

#### Changelog Synthesizer

- **Model**: Claude 4.5 Sonnet
- **Purpose**: Combines information to generate final documentation
- **Capabilities**: Audience adaptation, format compliance, version management,
  content curation, multi-period synthesis

### Replay Mode Agents (NEW)

#### Period Detector

- **Model**: Claude 4.5 Haiku (cost-optimized)
- **Purpose**: Fast period boundary calculation and release detection
- **Capabilities**: Auto-detect optimal period strategy, calculate calendar-aligned
  boundaries, identify releases, handle edge cases

#### Period Coordinator

- **Model**: Claude 4.5 Sonnet
- **Purpose**: Orchestrates multi-period workflow with parallel execution
- **Capabilities**: Batch processing, cache management, progress tracking,
  result aggregation, conflict resolution

## Configuration

Create `.changelog.yaml` in your repository root:

```yaml
# Changelog settings
changelog:
  conventional_commits: true      # Use conventional commit format
  include_commit_hash: false      # Include commit SHA in entries
  include_authors: true           # Credit contributors
  categories: # Categories to use
    - Added
    - Changed
    - Deprecated
    - Removed
    - Fixed
    - Security

# Release notes settings
release_notes:
  audience: "end-users"           # Target audience
  tone: "professional"            # Writing tone
  use_emoji: true                 # Include emoji
  max_entries: 5                  # Limit entries per category

# Version management
versioning:
  strategy: "semver"              # Version strategy
  auto_bump: # Auto-bump rules
    breaking: "major"
    features: "minor"
    fixes: "patch"

# AI analysis
ai_analysis:
  model: "claude-4-5-sonnet"      # Model for analysis
  analyze_unclear: true           # Analyze vague commits
  large_diff_threshold: 100       # Lines for "large" diff

# Release settings
release:
  allowed_branches: # Branches for releases
    - main
    - master
    - release/*
  version_files: # Files to update version
    - package.json
    - setup.py
    - VERSION
```

## Workflow Examples

### Weekly Changelog Update

```bash
# Every Monday morning
/changelog update

# Review generated entries
# Make any manual adjustments
# Commit the updated files
```

### Release Preparation

```bash
# 1. Update changelog with recent changes
/changelog update

# 2. Review and refine entries
/changelog review

# 3. Prepare release
/changelog-release minor

# 4. Push to repository
git push origin main --tags
```

### Retroactive Changelog Generation

```bash
# For existing project without changelogs
/changelog-init --from-beginning

# Review generated history
# Refine major milestones
# Commit the new files
```

## Historical Replay Mode

The replay feature generates changelogs by "replaying" your project's development history through time periods, creating changelog entries as if they were updated in real-time during each period. This is ideal for:

- **Retroactive changelog generation** with period-based organization
- **Long project histories** where a single flat changelog is overwhelming
- **Understanding project evolution** through temporal granularity
- **Release-based documentation** showing what changed between versions

### How It Works

1. **Period Detection** (Claude Haiku): Analyzes repository structure, commit frequency, and release tags to recommend optimal period strategy
2. **Boundary Calculation** (Claude Haiku): Calculates precise period boundaries with calendar alignment
3. **Parallel Analysis** (Claude Sonnet): Processes multiple periods concurrently (3 workers by default)
4. **Changelog Synthesis** (Claude Sonnet): Generates hybrid-format CHANGELOG.md with version sections containing period subsections

### Quick Start

```bash
# Interactive mode with auto-detection
/changelog-init --replay

# Specify period strategy
/changelog-init --replay --period weekly
/changelog-init --replay --period monthly
/changelog-init --replay --period by-release

# With custom workers for faster processing
/changelog-init --replay --period monthly --max-workers 5
```

### Example: Auto-Detection Workflow

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

⚙️ Executing parallel analysis (3 workers)...
[Progress] 63/63 periods analyzed - 100% - 3m 42s

✅ Generated CHANGELOG.md (1,847 lines, hybrid format)
✅ Generated RELEASE_NOTES.md (423 lines, period-aware)
✅ Configuration saved to .changelog.yaml
✅ Cache created: .changelog-cache/ (63 period analyses)
```

### Hybrid Format Output

The replay mode generates a hybrid-format CHANGELOG.md that combines version releases with period subsections:

```markdown
# Changelog

## [Unreleased]

### Week of November 11, 2025

#### Added
- Real-time notification system (#256)
- Advanced search filters (#252)

## [2.1.0] - 2024-10-27

### Week of October 21, 2024

#### Added
- REST API v2 with pagination (#234)
  - Backward compatible with v1

#### Changed
- **BREAKING:** JWT authentication

### Week of October 14, 2024

#### Fixed
- Race condition in file uploads (#245)

## [2.0.0] - 2024-09-30

### Month of September 2024

#### Added
- Complete UI redesign (#210)
- Dark mode support (#215)

#### Removed
- Python 3.7 support
```

### Period Strategies

| Strategy | Best For | Example Periods |
|----------|----------|-----------------|
| **daily** | Very active projects (5+ commits/day) | 2025-11-14, 2025-11-15 |
| **weekly** | Regular activity (15-30 commits/week) | Week of Nov 11, Week of Nov 4 |
| **monthly** | Moderate activity (20-40 commits/month) | November 2025, October 2025 |
| **quarterly** | Slow-moving projects (<20 commits/month) | Q4 2025, Q3 2025 |
| **by-release** | Tag-driven workflows | v2.1.0, v2.0.0, v1.5.0 |
| **auto** | Unknown - lets AI decide | Analyzes and recommends best fit |

### Configuration

Add replay settings to `.changelog.yaml`:

```yaml
replay:
  enabled: false
  default_strategy: "auto"  # auto | weekly | monthly | by-release

  auto_detection:
    min_releases_for_release_strategy: 3
    min_months_for_monthly_strategy: 6
    daily_if_commits_per_day_exceed: 5
    weekly_if_commits_per_week_exceed: 20

  calendar:
    week_start: "monday"
    use_calendar_months: true

  boundaries:
    boundary_handling: "inclusive_start"  # Include commits on start date
    unreleased_handling: "include"  # Partial final period → [Unreleased]

  filters:
    min_commits: 1
    skip_merge_only_periods: true

  output:
    hybrid_format: true
    include_period_headers: true
    period_header_format: "### {period_label}"
    replay_in_release_notes: true

  performance:
    enable_parallel: true
    max_concurrent_periods: 3  # 1-5 workers
    enable_cache: true
    cache_ttl_days: 0  # Never expire
```

### Performance

| Repository Size | Period Strategy | Periods | Workers | Estimated Time |
|----------------|-----------------|---------|---------|----------------|
| Small (100 commits) | Weekly | 15 | 3 | ~45 seconds |
| Medium (500 commits) | Weekly | 60 | 3 | ~4 minutes |
| Large (2000 commits) | Monthly | 36 | 3 | ~6 minutes |
| Large (2000 commits) | Monthly | 36 | 5 | ~4 minutes |
| Very Large (10k commits) | Quarterly | 20 | 5 | ~8 minutes |

**Optimization Tips:**
- Use caching (enabled by default) for instant regeneration
- Increase workers (up to 5) for faster processing
- Choose coarser periods (monthly vs weekly) for large histories
- First run is slower; subsequent runs use cached period analyses

### Edge Cases Handled

- **Empty Periods**: Automatically skipped (no changelog entry)
- **Merge-Only Periods**: Automatically skipped (configurable)
- **First Period with 100+ Commits**: Summarized instead of listing every commit
- **Partial Final Period**: Moved to [Unreleased] section
- **Multiple Tags in One Period**: Uses highest version number
- **Pre-Release Tags**: Separate entries (v2.0.0-beta, v2.0.0-rc1, v2.0.0)

### Advanced Usage

#### Regenerate Specific Period

```bash
/changelog-init --replay-period "2024-Q3" --force
```

#### Custom Worker Count

```bash
# Faster processing with more parallelism
/changelog-init --replay --period weekly --max-workers 5

# Conservative (low memory)
/changelog-init --replay --period monthly --max-workers 1
```

#### Disable Caching

```bash
# Force fresh analysis (slower)
/changelog-init --replay --period weekly --no-cache
```

## Best Practices

### Commit Messages

While the plugin can analyze unclear commits, clear messages improve results:

- Use conventional commits: `feat:`, `fix:`, `docs:`, etc.
- Include issue/PR references: `fixes #123`
- Mark breaking changes: `feat!:` or `BREAKING CHANGE:`

### Regular Updates

- Update changelogs frequently (weekly recommended)
- Don't wait until release to update documentation
- Review AI-generated content for accuracy

### Version Management

- Follow semantic versioning strictly
- Document breaking changes thoroughly
- Provide migration guides when needed

### Documentation Quality

- Keep technical details in `CHANGELOG.md`
- Focus on user value in `RELEASE_NOTES.md`
- Use consistent formatting and structure

## Advanced Features

### Monorepo Support

The plugin can handle monorepos with multiple packages:

```yaml
monorepo:
  enabled: true
  packages:
    - packages/core
    - packages/cli
    - packages/ui
  independent_versioning: true
```

### GitHub Integration

Automatically connect commits to GitHub artifacts for richer context:

#### Features

- **Intelligent Matching**: Links commits to Issues, Pull Requests, Projects V2, and Milestones
- **Multiple Strategies**:
  - Explicit references (`fixes #123`, `closes #456`)
  - Timestamp correlation (±14 days accounting for delays)
  - Semantic similarity (AI-powered content matching)
- **Composite Scoring**: Rewards commits matching multiple strategies with confidence bonuses
- **Smart Caching**: Local cache with configurable TTL to minimize API calls
- **Graceful Degradation**: Automatically disabled if prerequisites unavailable

#### Configuration

```yaml
integrations:
  github:
    matching:
      enabled: true
      cache_ttl_hours: 24
      time_window_days: 14
      confidence_threshold: 0.85

      fetch:
        issues: true
        pull_requests: true
        projects: true
        milestones: true

      strategies:
        explicit_reference: true
        timestamp_correlation: true
        semantic_similarity: true

      scoring:
        timestamp_and_semantic_bonus: 0.15
        all_strategies_bonus: 0.20

    references:
      # Technical changelog
      changelog:
        format: "detailed"  # Show all references
        show_issue_refs: true
        show_pr_refs: true
        show_project_refs: true

      # User-facing notes
      release_notes:
        format: "minimal"  # Just issue numbers
        show_issue_refs: true
        show_pr_refs: false
```

#### Prerequisites

- GitHub remote repository
- `gh` CLI installed and authenticated
- Internet connectivity for initial fetch

#### Example Output

**CHANGELOG.md (detailed format):**
```markdown
### Added
- REST API v2 with pagination support (#234, @dev1)
  [Closes: #189, #201 | PR: #234 | Project: Backend Roadmap | Milestone: v2.0.0]
```

**RELEASE_NOTES.md (minimal format):**
```markdown
#### ✨ Real-Time Notifications [#189]
Never miss important updates! We've added real-time notifications...
```

### Custom Categories

Define project-specific change categories:

```yaml
changelog:
  categories:
    - Added
    - Changed
    - Performance  # Custom category
    - Infrastructure  # Custom category
    - Fixed
    - Security
```

### Platform Integration

Generate platform-specific release notes:

```bash
/changelog-release --platform github
/changelog-release --platform gitlab
/changelog-release --platform jira
```

### Automation Hooks

Add pre/post-release scripts:

```bash
# .changelog-hooks/pre-release.sh
npm test
npm run build

# .changelog-hooks/post-release.sh
npm publish
curl -X POST https://deploy-webhook.com
```

## Troubleshooting

### Common Issues

**Q: The plugin doesn't detect my commits**

- Ensure you're on the correct branch
- Check if commits are pushed to remote
- Verify git history with `git log`

**Q: Generated descriptions are inaccurate**

- Provide clearer commit messages
- Use conventional commit format
- Review and edit generated content

**Q: Version bumping incorrect**

- Check `.changelog.yaml` configuration
- Ensure breaking changes are properly marked
- Use manual version specification if needed

### Debug Mode

Enable verbose output for troubleshooting:

```bash
/changelog --debug
```

## Performance Considerations

- **Large Repositories**: The plugin handles repos with 10,000+ commits
  efficiently
- **Token Usage**: Strategically uses Haiku (cost-optimized) for repetitive tasks
  and Sonnet (high-accuracy) for complex reasoning
- **Caching**: Results are cached to avoid redundant analysis; replay mode caches
  per-period for instant regeneration
- **Batch Processing**: Commits are analyzed in optimized batches
- **Parallel Execution** (Replay Mode): Processes multiple periods concurrently
  (3-5 workers) for faster large-scale analysis
- **Estimated Times** (Replay Mode):
  - Small repos (100 commits): ~45 seconds
  - Medium repos (500 commits): ~4 minutes
  - Large repos (2000 commits): ~6 minutes (3 workers) or ~4 minutes (5 workers)
  - Very large repos (10k commits): ~8 minutes with quarterly periods

## Privacy & Security

- All analysis happens locally or through Claude API
- No data is sent to third-party services
- Git history remains in your repository
- API keys are managed by Claude Code

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for
guidelines.

### Development Setup

```bash
# Clone the repository
git clone https://github.com/mtr/marketplace/changelog-manager.git

# Install dependencies
npm install

# Run tests
npm test

# Link for local development
ln -s $(pwd) ~/.claude/plugins/changelog-manager-dev
```

## Support

- **Documentation
  **: [Full documentation](https://docs.example.com/changelog-manager)
- **Issues
  **: [GitHub Issues](https://github.com/mtr/marketplace/changelog-manager/issues)
- **Discussions
  **: [GitHub Discussions](https://github.com/mtr/marketplace/changelog-manager/discussions)
- **Email**: martin@zemantiq.com

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Acknowledgments

- Inspired by [Keep a Changelog](https://keepachangelog.com)
- Built for [Claude Code](https://code.claude.com)
- Powered by Anthropic's Claude AI models

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for detailed version history.

---

**Created by Dr. Martin Thorsen Ranang
** | [GitHub](https://github.com/mtr) | [Website](https://zemantiq.com)
