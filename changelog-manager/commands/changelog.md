---
description: Initialize or update CHANGELOG.md and RELEASE_NOTES.md from git history
aliases: [ "cl", "change", "release" ]
---

# changelog

Intelligent management of CHANGELOG.md and RELEASE_NOTES.md files through
AI-powered git history analysis.

## Overview

The `/changelog` command provides a comprehensive workflow for maintaining both
developer-focused changelogs and user-facing release notes. It analyzes git
commits, groups related changes, and generates appropriate documentation for
different audiences.

## Usage

```bash
/changelog # Interactive mode - guides you through the process
/changelog init # Initialize both files for the first time
/changelog update # Update files with changes since last update
/changelog review # Review and refine existing entries
/changelog release [version] # Prepare a new release with version bump
```

## Workflow Phases

### Phase 1: Discovery & Analysis

- Detects existing CHANGELOG.md and RELEASE_NOTES.md files
- Identifies last update timestamp from file metadata
- Analyzes repository structure and conventions
- Determines version numbering scheme

### Phase 2: Git History Analysis

- Collects commits since last update (or all if initializing)
- Groups commits by:
    - Feature branches
    - Pull requests
    - Time proximity
    - Semantic similarity
- Identifies breaking changes, features, fixes, and improvements

### Phase 3: Commit Understanding

- For complex or unclear commits:
    - Analyzes code diffs with AI (Haiku model for efficiency)
    - Extracts semantic meaning from changes
    - Correlates with issue tracker references
    - Identifies user impact

### Phase 4: Change Categorization

- Organizes changes following Keep a Changelog convention:
    - Added: New features
    - Changed: Changes in existing functionality
    - Deprecated: Soon-to-be removed features
    - Removed: Removed features
    - Fixed: Bug fixes
    - Security: Security-related changes

### Phase 5: Documentation Generation

- Creates technical entries for CHANGELOG.md:
    - Comprehensive listing of all changes
    - Technical details and implementation notes
    - References to PRs, issues, and commits
    - Breaking change warnings

- Creates user-focused entries for RELEASE_NOTES.md:
    - High-level feature descriptions
    - User benefits and value propositions
    - Migration guides for breaking changes
    - Screenshots/examples where applicable

### Phase 6: Review & Refinement

- Presents generated content for review
- Allows interactive editing and refinement
- Validates version numbering (semantic versioning)
- Checks for consistency and completeness

### Phase 7: Finalization

- Writes updated files with proper formatting
- Creates git commit for documentation updates
- Tags release if version specified
- Generates release announcement template (optional)

## Interactive Example

```
You: /changelog

Claude: I'll help you manage your changelog and release notes. Let me analyze your repository...

📊 Repository Analysis:
- Found existing CHANGELOG.md (last updated: 2025-11-01)
- Found existing RELEASE_NOTES.md (last updated: 2025-11-01)
- Current version: v2.3.1
- 47 commits since last update

What would you like to do?
1. Update both files with recent changes
2. Prepare a new release (with version bump)
3. Review and refine existing entries
4. Generate release announcement

You: 1

Claude: Analyzing 47 commits since 2025-11-01...

🔍 Identified Changes:
- 3 new features
- 12 improvements
- 8 bug fixes
- 2 breaking changes
- 1 security update

Let me group these semantically and analyze their impact...

[Invokes git-history-analyzer agent]
[Invokes commit-analyst agent for complex changes]
[Invokes changelog-synthesizer agent]

📝 Generated CHANGELOG.md entries:

## [Unreleased]
### Added
- REST API v2 with pagination support (#234, @dev1)
- WebSocket real-time notifications (implements #189)
- Docker compose configuration for local development

### Changed
- **BREAKING:** Updated authentication to use JWT tokens instead of sessions
- Improved database query performance by 40% through indexing
- Migrated from Webpack to Vite for faster builds

### Fixed
- Memory leak in background job processor (#245)
- Incorrect timezone handling in scheduled tasks
- Race condition in concurrent file uploads

### Security
- Updated dependencies to patch CVE-2025-1234

📢 Generated RELEASE_NOTES.md entries:

## What's New

### 🚀 Real-time Notifications
Stay updated with instant WebSocket-powered notifications. No more manual refreshing!

### ⚡ 40% Faster Performance
We've optimized our database queries, making the app significantly more responsive.

### 🔒 Enhanced Security
Upgraded to JWT-based authentication for better security and seamless API access.

### 🐛 Stability Improvements
Fixed several bugs including memory leaks and timezone issues for a smoother experience.

⚠️ **Important:** This update changes how authentication works. You'll need to log in again after updating.

Would you like to:
1. Apply these changes
2. Edit before applying
3. Generate more detailed entries
4. Cancel

You: 1

Claude: ✅ Successfully updated:
- CHANGELOG.md (23 new entries added)
- RELEASE_NOTES.md (4 user-facing updates added)

Created commit: "docs: Update changelog and release notes for unreleased changes"

Next steps:
- Review the updated files
- Consider creating a release with `/changelog release v2.4.0`
- Share release notes with your team
```

## Configuration Options

Create `.changelog.yaml` in your repository root to customize behavior:

```yaml
# Changelog configuration
changelog:
  # Use conventional commits for automatic categorization
  conventional_commits: true

  # Include commit hashes in CHANGELOG.md
  include_commit_hash: true

  # Include author attribution
  include_authors: true

  # Group commits by these criteria
  grouping:
    - pull_request
    - feature_branch
    - semantic_similarity

  # Categories to use (Keep a Changelog format)
  categories:
    - Added
    - Changed
    - Deprecated
    - Removed
    - Fixed
    - Security

  # Breaking change indicators
  breaking_indicators:
    - "BREAKING CHANGE:"
    - "BREAKING:"
    - "!:" # Conventional commit breaking change

# Release notes configuration
release_notes:
  # Target audience
  audience: "end-users" # or "developers", "stakeholders"

  # Tone
  tone: "professional" # or "casual", "technical"

  # Include screenshots/gifs
  include_visuals: false

  # Emoji usage
  use_emoji: true

  # Maximum entries per category
  max_entries: 5

# Version management
versioning:
  # Semantic versioning strategy
  strategy: "semver" # or "calver", "custom"

  # Auto-bump rules
  auto_bump:
    breaking: "major"
    features: "minor"
    fixes: "patch"

# AI analysis settings
ai_analysis:
  # Model for commit analysis
  model: "claude-3-haiku" # Fast and efficient for code analysis

  # Analyze commits with unclear messages
  analyze_unclear: true

  # Analyze large diffs for better understanding
  analyze_large_diffs: true

  # Threshold for "large" diff (lines changed)
  large_diff_threshold: 100
```

## Agents Used

This command coordinates multiple specialized agents:

- **git-history-analyzer**: Examines commit history and groups related changes
- **commit-analyst**: Uses AI to understand complex commits and code changes
- **changelog-synthesizer**: Combines information to generate both technical and
  user-facing documentation

## Best Practices

1. **Regular Updates**: Run `/changelog update` frequently (e.g., weekly) to
   avoid large batches
2. **Review Generated Content**: Always review AI-generated entries for accuracy
3. **Maintain Consistency**: Use the same categorization and format across
   releases
4. **Version Strategically**: Follow semantic versioning for clear communication
5. **Separate Audiences**: Keep technical details in CHANGELOG.md, user value in
   RELEASE_NOTES.md

## Tips

- Use `--dry-run` flag to preview changes without writing files
- Run `/changelog review` before releases to ensure quality
- Configure `.changelog.yaml` for project-specific conventions
- Tag releases in git after updating documentation
- Consider automating with git hooks or CI/CD pipelines

## Related Commands

- `/changelog init` - Initialize changelog files for a new project
- `/changelog release` - Prepare a new release with version bump
- `/git-history` - Explore git history in detail
- `/version` - Manage version numbers
