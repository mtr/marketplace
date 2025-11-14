# Changelog Manager Plugin - Architecture & Deployment Guide

## Plugin Architecture

The changelog-manager plugin follows a sophisticated multi-agent architecture designed for efficient and intelligent changelog generation:

```
┌─────────────────────────────────────────────────────────────┐
│                     User Interface                          │
│                   /changelog Commands                       │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│                  Command Orchestration                      │
│              /changelog, /changelog-init,                   │
│                  /changelog-release                         │
└──────┬──────────────┬────────────────┬─────────────────────┘
       │              │                │
       ▼              ▼                ▼
┌──────────────┐ ┌──────────────┐ ┌────────────────────────┐
│ Git History  │ │   Commit     │ │ Changelog Synthesizer  │
│  Analyzer    │ │   Analyst    │ │      (Sonnet)          │
│  (Sonnet)    │ │   (Sonnet)   │ │                        │
├──────────────┤ ├──────────────┤ ├────────────────────────┤
│ - Extract    │ │ - Analyze    │ │ - Generate CHANGELOG   │
│   commits    │ │   diffs      │ │ - Generate RELEASE_    │
│ - Group      │ │ - Understand │ │   NOTES.md             │
│   changes    │ │   purpose    │ │ - Version mgmt         │
│ - Detect     │ │ - Assess     │ │ - Format compliance    │
│   patterns   │ │   impact     │ │ - Audience adaptation  │
└──────┬───────┘ └──────────────┘ └────────────────────────┘
       │                │                │
       │ (Optional)     │                │
       ▼                │                │
┌──────────────────────┐│                │
│  GitHub Matcher      ││                │
│     (Sonnet)         ││                │
├──────────────────────┤│                │
│ - Fetch Issues/PRs   ││                │
│ - Match by reference ││                │
│ - Timestamp correl.  ││                │
│ - Semantic matching  ││                │
│ - Composite scoring  ││                │
│ - Local caching      ││                │
└──────┬───────────────┘│                │
       │                │                │
       └────────────────┼────────────────┘
                       ▼
      ┌────────────────────────────────┐
      │      Git Repository &          │
      │  File System & GitHub API      │
      └────────────────────────────────┘
```

## Core Components

### 1. Commands Layer
- **Purpose**: User interaction and workflow orchestration
- **Components**:
  - `/changelog`: Main interactive command
  - `/changelog-init`: Initialization wizard
  - `/changelog-release`: Release preparation

### 2. Agent Layer
- **Purpose**: Specialized AI-powered analysis and generation
- **Components**:
  - **git-history-analyzer**: Repository and commit pattern analysis (Sonnet)
  - **commit-analyst**: Deep code diff understanding (Sonnet)
  - **github-matcher**: Optional GitHub artifact matching with composite scoring (Sonnet)
  - **changelog-synthesizer**: Documentation generation (Sonnet)

### 3. Configuration Layer
- **Purpose**: Project-specific customization
- **File**: `.changelog.yaml`
- **Features**: Extensive customization of behavior, format, and integration

### 4. Output Layer
- **Purpose**: Generated documentation
- **Files**:
  - `CHANGELOG.md`: Technical changelog
  - `RELEASE_NOTES.md`: User-facing notes
  - Version files: Updated automatically
  - Release artifacts: Tags, branches, announcements

## Model Strategy

The plugin uses Claude 4.5 Sonnet for all analysis and generation tasks:

1. **Claude 4.5 Sonnet** (Primary Model)
   - Used for: All analysis, pattern recognition, and synthesis tasks
   - Agents: git-history-analyzer, commit-analyst, changelog-synthesizer
   - Rationale: Comprehensive reasoning, deep code understanding, and high-quality generation

## Deployment Instructions

### Method 1: Via Claude Code Plugin System

```bash
# 1. Add the marketplace
/plugin marketplace add mtr/marketplace/changelog-manager

# 2. Install the plugin
/plugin install changelog-manager

# 3. Verify installation
/help changelog
```

### Method 2: Manual Installation

```bash
# 1. Clone the repository
git clone https://github.com/mtr/marketplace/changelog-manager.git ~/.claude/plugins/changelog-manager

# 2. Restart Claude Code
# The plugin will be automatically detected

# 3. Verify installation
/help changelog
```

### Method 3: From Local Directory (Development)

```bash
# 1. Navigate to plugin directory
cd /home/claude/changelog-manager

# 2. Create symbolic link
ln -s $(pwd) ~/.claude/plugins/changelog-manager

# 3. Restart Claude Code
```

## Publishing the Plugin

### Step 1: Prepare Repository

```bash
# Create GitHub repository
git init
git add .
git commit -m "feat: Initial release of changelog-manager plugin"
git branch -M main
git remote add origin https://github.com/mtr/marketplace/changelog-manager.git
git push -u origin main

# Create release tag
git tag -a v1.0.0 -m "Release v1.0.0: Production-ready changelog management"
git push origin v1.0.0
```

### Step 2: Create Marketplace Repository

```bash
# Create separate marketplace repository
cd /home/claude/changelog-manager-marketplace
git init
git add .
git commit -m "feat: Add changelog-manager to marketplace"
git remote add origin https://github.com/mtr/marketplace/changelog-manager-marketplace.git
git push -u origin main
```

### Step 3: Distribution

Share the installation command:
```bash
/plugin marketplace add mtr/marketplace/changelog-manager-marketplace
/plugin install changelog-manager
```

## Configuration Best Practices

### For Open Source Projects

```yaml
# .changelog.yaml
changelog:
  include_authors: true
  include_references: true
  
release_notes:
  audience: "developers"
  tone: "technical"
  include_contributors: true
```

### For Enterprise Projects

```yaml
# .changelog.yaml
changelog:
  include_commit_hash: true
  breaking_indicators:
    - "BREAKING:"
    - "MIGRATION REQUIRED:"
    
release_notes:
  audience: "stakeholders"
  tone: "professional"
  
integrations:
  jira:
    enabled: true
```

### For Solo Developers

```yaml
# .changelog.yaml
changelog:
  conventional_commits: false
  include_authors: false
  
release_notes:
  audience: "end-users"
  tone: "casual"
  use_emoji: true
  
versioning:
  strategy: "calver"
```

## Performance Optimization

### Token Usage Optimization

1. **Efficient Context Management**: Focused analysis windows for optimal token usage
2. **Batch Processing**: Groups similar commits
3. **Caching**: Avoids re-analyzing unchanged commits
4. **Smart Sampling**: Analyzes representative diffs for large changes

### Processing Speed

- Small repo (<100 commits): ~5-10 seconds
- Medium repo (100-1000 commits): ~30-60 seconds
- Large repo (1000+ commits): ~2-5 minutes
- With caching: 50-80% faster on subsequent runs

## GitHub Integration

The plugin optionally integrates with GitHub to enrich changelogs with artifact references:

### Architecture

```
Commit Analysis Flow:
1. Git History Analyzer extracts and groups commits
2. If GitHub integration enabled:
   a. GitHub Matcher fetches Issues/PRs/Projects/Milestones
   b. Matches commits using multiple strategies
   c. Calculates composite confidence scores
   d. Returns enriched commit data
3. Changelog Synthesizer formats references per document type
```

### Matching Strategies

**1. Explicit Reference (100% confidence)**
- Parses commit messages for `#123`, `fixes #456`, `closes #789`
- Instant match, no further analysis needed

**2. Timestamp Correlation (40-85% confidence)**
- Compares commit timestamp with artifact activity
- ±14 days time window accounting for process delays
- Proximity-based scoring

**3. Semantic Similarity (40-95% confidence)**
- AI-powered content matching
- Compares commit message/diff with issue/PR description
- Continuous score scaled from raw similarity

### Composite Scoring Bonuses

- **Timestamp + Semantic**: +15% when both strategies match
- **Timestamp + Branch**: +10% when commit on PR branch
- **All Strategies**: +20% when 3+ strategies align

### Cache Architecture

```
~/.claude/changelog-manager/cache/{repo-hash}/
├── issues.json          # All issues with full metadata
├── pull_requests.json   # All PRs with full metadata
├── projects.json        # GitHub Projects V2 data
├── milestones.json      # Milestone information
└── metadata.json        # Cache metadata (timestamps, ttl)
```

**Invalidation Strategy:**
- Time-based: 24-hour default TTL (configurable)
- Session-based: Check at start of each Claude session
- Manual: `--force-update` flag
- Selective: Only refetch stale artifact types

### Configuration

See `.changelog.yaml` for complete configuration options:
- Enable/disable integration
- Configure confidence thresholds
- Set time windows and cache TTL
- Choose matching strategies
- Customize reference display per document type

## Integration Points

### CI/CD Integration

```yaml
# .github/workflows/changelog.yml
name: Update Changelog
on:
  push:
    branches: [main]
jobs:
  changelog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Update Changelog
        run: |
          claude-code --command "/changelog update"
      - name: Commit changes
        run: |
          git add CHANGELOG.md RELEASE_NOTES.md
          git commit -m "docs: Update changelog"
          git push
```

### Git Hooks

```bash
# .git/hooks/pre-push
#!/bin/bash
echo "Updating changelog..."
claude-code --command "/changelog update --dry-run"
```

## Troubleshooting

### Common Issues

1. **Plugin not loading**
   ```bash
   claude --debug
   # Check for JSON syntax errors in plugin.json
   ```

2. **Commands not appearing**
   ```bash
   # Ensure commands/ directory is at plugin root
   ls ~/.claude/plugins/changelog-manager/commands/
   ```

3. **Agent errors**
   ```bash
   # Check agent file syntax
   /changelog --debug
   ```

## Security Considerations

1. **API Security**: All Claude API calls handled by Claude Code
2. **Repository Access**: Only local git operations
3. **No External Services**: No data sent to third parties
4. **Token Management**: Efficient token usage with optimized context windows
5. **Configuration**: Sensitive data should not be in .changelog.yaml

## Future Enhancements

Potential improvements for future versions:

1. **Enhanced Integrations**
   - Confluence documentation export
   - Teams/Slack notifications
   - Automatic PR/MR descriptions

2. **Advanced Analysis**
   - Security vulnerability detection
   - Performance impact analysis
   - Dependency change tracking

3. **Visualization**
   - Change frequency graphs
   - Contributor statistics
   - Release velocity metrics

4. **Workflow Features**
   - Approval workflows
   - Multi-language changelog generation
   - Template marketplace

## Support & Contribution

- **Issues**: Report bugs via GitHub Issues
- **Features**: Request features via GitHub Discussions
- **Contributing**: PRs welcome! See CONTRIBUTING.md
- **Support**: martin@zemantiq.com

## License

MIT License - See LICENSE file

---

**Plugin Version**: 1.1.0
**Claude Code Compatibility**: >=1.0.0
**Author**: Dr. Martin Thorsen Ranang
**Last Updated**: November 14, 2025
