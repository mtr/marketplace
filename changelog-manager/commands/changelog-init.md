---
description: Initialize CHANGELOG.md and RELEASE_NOTES.md for a new project or retroactively from git history
aliases: ["cl-init"]
---

# changelog-init

Initialize changelog documentation for a project by analyzing entire git history and generating both developer and user-facing documentation files.

## Usage

```bash
/changelog-init                    # Interactive initialization
/changelog-init --from-beginning  # Analyze entire git history
/changelog-init --from-tag v1.0.0 # Start from specific version
/changelog-init --empty           # Create empty template files
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

## Options

- `--from-beginning`: Analyze entire repository history
- `--from-tag <tag>`: Start from specific git tag
- `--empty`: Create template files without historical content
- `--config-only`: Only generate .changelog.yaml configuration
- `--force`: Overwrite existing files (backup created)

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
```

## Integration

This command works with:
- `/changelog update` - Add new changes
- `/changelog release` - Prepare releases
- Git hooks - Automate updates
