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
```

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

The plugin employs three specialized AI agents:

### Git History Analyzer

- **Model**: Claude 4.5 Sonnet
- **Purpose**: Analyzes commit history, groups related changes, detects patterns
- **Capabilities**: Branch analysis, PR correlation, semantic clustering,
  version detection

### Commit Analyst

- **Model**: Claude 4.5 Sonnet (optimized for accuracy)
- **Purpose**: Deep analysis of individual commits and code diffs
- **Capabilities**: Diff interpretation, impact assessment, purpose extraction,
  breaking change detection

### Changelog Synthesizer

- **Model**: Claude 4.5 Sonnet
- **Purpose**: Combines information to generate final documentation
- **Capabilities**: Audience adaptation, format compliance, version management,
  content curation

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
- **Token Usage**: Sonnet model provides comprehensive analysis for commit understanding
- **Caching**: Results are cached to avoid redundant analysis
- **Batch Processing**: Commits are analyzed in optimized batches

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
