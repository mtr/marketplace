# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-11-14

### Added

- **Changelog Manager Plugin** - Complete AI-powered changelog and release notes generation system ([bb65b47], @mtr)
  - Multi-agent architecture with three specialized agents:
    - `git-history-analyzer` (Claude Sonnet 4.5): Examines commit history and groups related changes
    - `commit-analyst` (Claude Haiku 4.5): Analyzes individual commits and code diffs with 70% token cost reduction
    - `changelog-synthesizer` (Claude Sonnet 4.5): Generates both technical and user-facing documentation
  - Three slash commands for complete workflow:
    - `/changelog`: Interactive changelog management
    - `/changelog-init`: Initialize files for first time
    - `/changelog-release`: Prepare versioned release with bumping
  - Configuration system via `.changelog.yaml` templates
  - Full compliance with Keep a Changelog and Semantic Versioning standards
  - Local-only git analysis with no external dependencies
  - Support for monorepo architectures
  - Components added (14 files, 2,997 lines):
    - 3 AI agent definitions
    - 3 command implementations
    - Configuration template
    - Complete documentation suite (README, ARCHITECTURE, RELEASE_NOTES, CHANGES)
    - MIT License

- **Plugin Marketplace Infrastructure** - Foundation for hosting Claude Code plugins ([9951aa4], @mtr)
  - Marketplace metadata (`.claude-plugin/marketplace.json`)
  - Repository structure for multi-plugin hosting
  - Root documentation

### Changed

- **Upgraded to Claude 4.5 Models** - Enhanced AI performance across all agents ([1aa8fe5], @mtr)
  - Updated `git-history-analyzer` to Claude Sonnet 4.5
  - Updated `commit-analyst` to Claude Haiku 4.5
  - Updated `changelog-synthesizer` to Claude Sonnet 4.5
  - Improved accuracy in commit analysis and natural language generation
  - Updated documentation to reflect model capabilities

- **Optimized Default Configuration** - Better out-of-box experience ([1aa8fe5], @mtr)
  - Changed `max_changelog_entries` from 20 to 0 (unlimited) for complete history
  - Increased `worker_threads` from 3 to 5 for faster parallel commit analysis
  - Updated support contact email

- **Improved Documentation Formatting** - Enhanced readability and consistency ([1aa8fe5], [991e6e4], @mtr)
  - Standardized line formatting across all markdown files
  - Unified author attribution as "Dr. Martin Thorsen Ranang"
  - Improved markdown syntax consistency
  - Enhanced visual presentation

### Repository Statistics

- **Contributors**: 1 (Dr. Martin Thorsen Ranang)
- **Commits**: 4
- **Files Changed**: 16
- **Lines Added**: 3,281
- **Lines Removed**: 80
- **Net Change**: +3,201 lines
- **Development Time**: Initial rapid development session (~50 minutes)

---

## Version Comparison

[1.0.0]: https://github.com/mtr/marketplace/compare/9951aa4...991e6e4

[bb65b47]: https://github.com/mtr/marketplace/commit/bb65b47
[1aa8fe5]: https://github.com/mtr/marketplace/commit/1aa8fe5
[991e6e4]: https://github.com/mtr/marketplace/commit/991e6e4
[9951aa4]: https://github.com/mtr/marketplace/commit/9951aa4
