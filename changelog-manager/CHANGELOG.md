# Changelog

All notable changes to the changelog-manager plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2025-11-14

### Added

- GitHub integration with intelligent artifact matching
  - Automatically matches commits to GitHub Issues, Pull Requests, Projects V2, and Milestones
  - Multiple matching strategies: explicit references, timestamp correlation, and AI-powered semantic similarity
  - Composite confidence scoring with strategy-based bonuses (+15% timestamp+semantic, +10% timestamp+branch, +20% all strategies)
  - Smart local caching with configurable TTL (default 24 hours) to minimize GitHub API calls
  - Cache stored at `~/.claude/changelog-manager/cache/{repo-hash}/` with per-artifact-type invalidation
  - Graceful degradation when GitHub remote not detected or `gh` CLI unavailable
- New `github-matcher` agent (Claude 4.5 Sonnet) for intelligent artifact matching
  - Explicit reference matching (100% confidence for `#123`, `fixes #456`, etc.)
  - Timestamp correlation (40-85% confidence based on ±14 days proximity)
  - Semantic similarity matching (40-95% confidence scaled from AI analysis)
  - Configurable confidence threshold (default 85%)
- Configurable GitHub reference display per document type
  - CHANGELOG.md: Detailed format showing issues, PRs, projects, milestones
  - RELEASE_NOTES.md: Minimal format showing only issue numbers
  - Format options: detailed, inline, minimal, none
  - Per-artifact-type visibility controls (show/hide issues, PRs, projects, milestones)
- Extended `.changelog.yaml` configuration with comprehensive GitHub integration settings
  - Enable/disable matching integration
  - Configure time windows, confidence thresholds, and cache TTL
  - Select active matching strategies
  - Customize scoring bonuses for composite matches
  - Set semantic similarity parameters (min score, context length)
  - Configure reference display per document type

### Changed

- Updated all agent model references from Claude 3.5 Sonnet to Claude 4.5 Sonnet
- Updated `git-history-analyzer` to optionally invoke `github-matcher` for enrichment when GitHub integration enabled
- Enhanced `changelog-synthesizer` with GitHub reference formatting templates and per-document configuration support
- Improved `.changelog.yaml` template with extensive GitHub integration documentation

### Documentation

- Added comprehensive GitHub Integration section to README.md with features, configuration, prerequisites, and examples
- Updated ARCHITECTURE.md with:
  - New architecture diagram showing optional `github-matcher` agent
  - GitHub Integration section documenting matching strategies, composite scoring, and cache architecture
  - Updated Core Components to include `github-matcher` agent
  - Updated plugin version to 1.1.0
- Enhanced plugin.json description to mention GitHub integration
- Added "github" and "issue-tracking" keywords to plugin metadata

## [1.0.0] - 2025-11-13

### Added

- Initial release of changelog-manager plugin
- Three specialized AI agents using Claude 4.5 Sonnet:
  - `git-history-analyzer`: Extracts, groups, and categorizes commits
  - `commit-analyst`: Analyzes individual commit diffs
  - `changelog-synthesizer`: Generates CHANGELOG.md and RELEASE_NOTES.md
- Three slash commands:
  - `/changelog`: Interactive changelog management
  - `/changelog-init`: Initialize changelogs for new or existing projects
  - `/changelog-release`: Prepare releases with version bumping
- Dual documentation generation:
  - CHANGELOG.md: Technical, developer-focused changelog following Keep a Changelog format
  - RELEASE_NOTES.md: User-friendly release notes with emoji and accessible language
- Intelligent commit grouping strategies:
  - Pull request correlation
  - Feature branch analysis
  - Semantic similarity clustering
  - Time proximity grouping
- Breaking change detection via conventional commits (!, BREAKING CHANGE:)
- Semantic versioning support with auto-bump rules
- Monorepo support with per-package changelogs
- Configurable via `.changelog.yaml` template
- Standards compliance:
  - Keep a Changelog format
  - Semantic Versioning
  - Conventional Commits (optional)

### Documentation

- Comprehensive README.md with installation, usage, and examples
- ARCHITECTURE.md documenting plugin design and deployment
- RELEASE_NOTES.md with user-facing release information
- Configuration template with extensive inline documentation

[Unreleased]: https://github.com/mtr/marketplace/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/mtr/marketplace/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/mtr/marketplace/releases/tag/v1.0.0
