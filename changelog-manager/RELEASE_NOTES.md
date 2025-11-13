# Release Notes

## Version 1.0.0 - November 13, 2025

### 🎉 Introducing Changelog Manager

We're excited to announce the first stable release of Changelog Manager - your intelligent companion for maintaining beautiful, informative changelogs and release notes!

### ✨ What's New

#### 🤖 AI-Powered Understanding
Ever written a commit message like "fix stuff" and regretted it later? Changelog Manager uses advanced AI to understand what your commits actually do by analyzing the code changes themselves. No more manually deciphering cryptic commit messages!

#### 📚 Dual Documentation
Automatically generate two types of documentation from the same source:
- **Technical changelogs** for your development team with all the implementation details
- **User-friendly release notes** that focus on value and benefits for your users

Your stakeholders get what they need, in the language they understand.

#### 🎯 Smart Categorization
The plugin intelligently groups and categorizes your changes:
- Groups related commits from the same feature or PR
- Identifies breaking changes automatically
- Categorizes changes following industry standards
- Prioritizes by user impact

#### ⚡ Lightning-Fast Analysis
Using an optimized AI model (Claude Haiku) for commit analysis means:
- Process hundreds of commits in seconds
- Minimal API costs
- Efficient batch processing
- Smart caching for repeated analysis

#### 🚀 Release Automation
Preparing a release is now a breeze:
- Interactive release wizard guides you through the process
- Automatic version bumping based on your changes
- Git tags and release branches created automatically
- Platform-specific release notes for GitHub, GitLab, and more

### 🛠️ Getting Started

1. **Install the plugin:**
   ```bash
   /plugin install changelog-manager
   ```

2. **Initialize your changelogs:**
   ```bash
   /changelog-init
   ```

3. **Start tracking changes:**
   ```bash
   /changelog update
   ```

That's it! You now have professional changelog management at your fingertips.

### 💡 Use Cases

#### For Open Source Projects
- Keep contributors and users informed about changes
- Generate release notes for GitHub releases automatically
- Maintain transparency with comprehensive changelogs

#### For Enterprise Teams
- Standardize release documentation across projects
- Reduce time spent on release preparation
- Ensure compliance with documentation requirements

#### For Solo Developers
- Never forget what changed between versions
- Professional documentation with minimal effort
- Focus on coding, not documentation

### 📋 Key Features

- **Incremental Updates**: Only processes new commits since last update
- **Standards Compliance**: Follows Keep a Changelog and Semantic Versioning
- **Customizable**: Extensive configuration options to match your workflow
- **Multi-language**: Understands code changes in all major programming languages
- **Monorepo Support**: Handle multiple packages in a single repository
- **Platform Integration**: Works with GitHub, GitLab, Jira, and more

### 🔧 Configuration

Customize the plugin to match your workflow with a simple YAML configuration file. Control everything from commit categorization to AI analysis thresholds.

### 🚦 System Requirements

- Claude Code installed and configured
- Git repository with commit history
- No additional dependencies required!

### 📚 Documentation

- [Quick Start Guide](https://github.com/mtr/marketplace/changelog-manager#quick-start)
- [Configuration Reference](https://github.com/mtr/marketplace/changelog-manager#configuration)
- [Command Documentation](https://github.com/mtr/marketplace/changelog-manager#commands)

### 🤝 Support

Having issues or questions?
- 📧 Email: ranang+marketplace@gmail.com
- 🐛 Issues: [GitHub Issues](https://github.com/mtr/marketplace/changelog-manager/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/mtr/marketplace/changelog-manager/discussions)

### 🙏 Acknowledgments

Special thanks to the Claude Code team for making this plugin possible, and to the Keep a Changelog community for establishing the standards we follow.

---

**Ready to revolutionize your changelog workflow?** Install Changelog Manager today and never manually write release notes again!

```bash
/plugin install changelog-manager
```

*Happy releasing! 🚀*
