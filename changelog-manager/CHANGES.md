# Changelog

All notable changes to the changelog-manager plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2025-11-13

### Added
- Core `/changelog` command with interactive workflow for managing both CHANGELOG.md and RELEASE_NOTES.md files
- `/changelog-init` command for initializing changelog files in new or existing projects
  - Support for analyzing entire git history retroactively
  - Template generation for new projects
  - Automatic version detection from package files
- `/changelog-release` command for orchestrating release preparation
  - Interactive release wizard with pre-release checks
  - Automatic version bumping following semver
  - Git tag and release branch creation
  - Platform-specific release note generation
- Three specialized AI agents for intelligent processing:
  - **git-history-analyzer**: Examines commit history using Claude 3.5 Sonnet
    - Groups commits by PR, feature branch, and semantic similarity
    - Detects breaking changes and version patterns
    - Supports monorepo structures
  - **commit-analyst**: Deep analysis using Claude 3 Haiku for efficiency
    - Understands vague commit messages through diff analysis
    - Identifies hidden breaking changes
    - Extracts semantic meaning from code changes
  - **changelog-synthesizer**: Generates final documentation using Claude 3.5 Sonnet
    - Creates audience-appropriate content
    - Manages version continuity
    - Ensures format compliance
- Comprehensive configuration system via `.changelog.yaml`
  - Customizable categorization and grouping strategies
  - Configurable AI analysis thresholds
  - Platform integration settings
  - Monorepo support configuration
- Multi-language code understanding across major programming languages
- Intelligent commit grouping using multiple strategies:
  - Pull request correlation
  - Feature branch detection
  - Semantic clustering
  - Time proximity analysis
- Automatic breaking change detection through:
  - Conventional commit markers
  - API signature analysis
  - Configuration schema changes
- Platform integrations for GitHub, GitLab, and Jira
- Pre and post-release hooks for automation
- Batch processing optimization for large repositories
- Caching system for improved performance
- Support for pre-release versions (alpha, beta, rc)
- Migration guide generation for breaking changes

### Security
- All processing happens locally or through Claude API
- No third-party services receive repository data
- API keys managed securely by Claude Code

## [0.9.0-beta] - 2025-11-10

### Added
- Beta release with core functionality
- Basic commit analysis features
- Initial agent implementation

### Changed
- Refactored agent communication protocol
- Improved error handling in git operations

### Fixed
- Issue with detecting merge commits
- Incorrect categorization of dependency updates

## [0.8.0-alpha] - 2025-11-05

### Added
- Initial alpha release
- Proof of concept for AI-powered changelog generation
- Basic git history analysis

[Unreleased]: https://github.com/mtr/marketplace/changelog-manager/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/mtr/marketplace/changelog-manager/compare/v0.9.0-beta...v1.0.0
[0.9.0-beta]: https://github.com/mtr/marketplace/changelog-manager/compare/v0.8.0-alpha...v0.9.0-beta
[0.8.0-alpha]: https://github.com/mtr/marketplace/changelog-manager/releases/tag/v0.8.0-alpha
