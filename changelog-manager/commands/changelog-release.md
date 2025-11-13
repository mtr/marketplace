---
description: Prepare a new release by updating changelogs, bumping version, and creating release artifacts
aliases: ["cl-release", "release-prep"]
---

# changelog-release

Orchestrate the complete release preparation process including changelog finalization, version bumping, and release artifact generation.

## Usage

```bash
/changelog-release                    # Interactive release wizard
/changelog-release patch              # Patch version bump (x.x.Z)
/changelog-release minor              # Minor version bump (x.Y.0)
/changelog-release major              # Major version bump (X.0.0)
/changelog-release v2.5.0            # Specific version
/changelog-release --pre-release rc1  # Release candidate
```

## Release Workflow

### Phase 1: Pre-Release Checks
- Verify all tests pass
- Check for uncommitted changes
- Validate changelog entries
- Confirm version bump strategy
- Review breaking changes

### Phase 2: Version Management
- Calculate new version number
- Update version in all relevant files:
  - package.json
  - setup.py / pyproject.toml
  - VERSION file
  - Source code constants
- Update changelog headers

### Phase 3: Documentation Finalization
- Move "Unreleased" changes to versioned section
- Generate release summary
- Create migration guide (if breaking changes)
- Generate announcement template
- Update comparison links

### Phase 4: Release Artifacts
- Create git commit for release
- Generate git tag
- Create release branch (if applicable)
- Prepare distribution packages
- Generate release notes for platforms

### Phase 5: Post-Release Setup
- Create new "Unreleased" section
- Set up next development cycle
- Generate release checklist
- Create follow-up tasks

## Options

- `--dry-run`: Preview changes without applying
- `--no-tag`: Skip git tag creation
- `--no-commit`: Skip git commit
- `--pre-release <tag>`: Create pre-release (alpha, beta, rc)
- `--skip-tests`: Skip test verification
- `--announcement`: Generate announcement template

## Interactive Example

```bash
You: /changelog-release

Claude: 🚀 Release Preparation Wizard

📊 Current Status:
- Current version: v2.3.1
- Unreleased changes: 23 items
  - 3 new features (suggests minor bump)
  - 12 improvements
  - 8 bug fixes
  - 0 breaking changes

Suggested version: v2.4.0

What version would you like to release?
1. v2.4.0 (recommended - minor)
2. v2.3.2 (patch)
3. v3.0.0 (major)
4. Custom version
5. Pre-release (rc, beta, alpha)

You: 1

Claude: ✅ Preparing release v2.4.0...

🔍 Running pre-release checks:
- ✅ All tests passing
- ✅ No uncommitted changes
- ✅ Changelog entries valid
- ✅ No unresolved TODOs in code

📝 Updating version files:
- ✅ package.json: 2.3.1 → 2.4.0
- ✅ src/version.py: Updated VERSION constant
- ✅ README.md: Updated version badge

📚 Finalizing documentation:
- ✅ CHANGELOG.md: Moved 23 unreleased items to v2.4.0
- ✅ RELEASE_NOTES.md: Generated user-friendly summary
- ✅ Created release announcement template

Would you like to:
1. Review changes before committing
2. Proceed with release
3. Generate additional artifacts
4. Cancel

You: 1

Claude: 📄 Release Changes Preview:

**CHANGELOG.md excerpt:**
```markdown
## [2.4.0] - 2025-11-13

### Added
- REST API v2 with pagination support (#234)
- WebSocket real-time notifications (#240)
- Docker Compose development environment
```

**RELEASE_NOTES.md excerpt:**
```markdown
## Version 2.4.0 - November 13, 2025

### ✨ What's New
- Real-time notifications keep you updated instantly
- Lightning-fast API with new pagination
- Easier development setup with Docker
```

**Files to be modified:**
- CHANGELOG.md
- RELEASE_NOTES.md
- package.json
- src/version.py

Continue with release? (yes/no)

You: yes

Claude: 🎯 Executing release...

✅ Documentation updated
✅ Version files updated
✅ Commit created: "chore(release): Prepare v2.4.0"
✅ Tag created: v2.4.0
✅ Release branch created: release/v2.4.0

📋 Release Checklist:
□ Push changes: `git push origin main --tags`
□ Create GitHub release
□ Deploy to production
□ Announce to team/users
□ Monitor for issues

📢 Announcement template saved to: RELEASE_ANNOUNCEMENT.md

🎉 Release v2.4.0 prepared successfully!
```

## Version Strategies

### Semantic Versioning
```
MAJOR.MINOR.PATCH

- MAJOR: Breaking changes
- MINOR: New features, backwards compatible
- PATCH: Bug fixes, backwards compatible
```

### Pre-releases
```
v2.4.0-rc.1   # Release candidate
v2.4.0-beta.2 # Beta version
v2.4.0-alpha.3 # Alpha version
```

### Calendar Versioning
```
2025.11.13    # Year.Month.Day
2025.11       # Year.Month
25.11.1       # YY.MM.Patch
```

## Platform Integration

### GitHub Release
```bash
# Generates GitHub-compatible release notes
/changelog-release --platform github
```

### GitLab Release
```bash
# Generates GitLab release format
/changelog-release --platform gitlab
```

### npm Publication
```bash
# Prepares for npm publish
/changelog-release --platform npm
```

## Configuration

Customize in `.changelog.yaml`:

```yaml
release:
  # Branches allowed for releases
  allowed_branches:
    - main
    - master
    - release/*
  
  # Required checks before release
  pre_release_checks:
    - tests_pass
    - no_uncommitted
    - changelog_valid
  
  # Files to update version in
  version_files:
    - package.json
    - setup.py
    - src/version.py
    - README.md
  
  # Post-release actions
  post_release:
    - create_github_release
    - notify_slack
    - trigger_deployment
```

## Automation Hooks

### Pre-release Hook
```bash
#!/bin/bash
# .changelog-hooks/pre-release.sh
echo "Running pre-release checks..."
npm test
npm run lint
```

### Post-release Hook
```bash
#!/bin/bash
# .changelog-hooks/post-release.sh
echo "Triggering deployment..."
curl -X POST https://deploy.example.com/webhook
```

## Best Practices

1. **Always review** generated changes before committing
2. **Run tests** before releasing
3. **Tag releases** for easy rollback
4. **Document breaking changes** thoroughly
5. **Announce releases** to stakeholders
6. **Monitor** post-release for issues

## Related Commands

- `/changelog update` - Update with recent changes
- `/version` - Manage version numbers
- `/git tag` - Create git tags
- `/release-notes` - Generate platform-specific notes
