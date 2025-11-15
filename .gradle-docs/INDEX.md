# Bearsampp Module Memcached - Documentation Index

Complete documentation for the Bearsampp Module Memcached build system.

## Documentation Structure

```
.gradle-docs/
├── INDEX.md                  # This file - Documentation index
├── README.md                 # Main documentation and quick start
├── TASKS.md                  # Complete task reference
├── CONFIGURATION.md          # Configuration guide
├── RELEASE-PROCESS.md        # Release process guide
└── MIGRATION-GUIDE.md        # Migration guide from Ant to Gradle
```

## Quick Navigation

### Getting Started

| Document                                          | Description                                    | Audience                |
|--------------------------------------------------|------------------------------------------------|-------------------------|
| [README.md](README.md)                           | Main documentation and quick start             | All users               |

### Reference Documentation

| Document                                          | Description                                    | Audience                |
|--------------------------------------------------|------------------------------------------------|-------------------------|
| [TASKS.md](TASKS.md)                             | Complete task reference                        | Developers              |
| [CONFIGURATION.md](CONFIGURATION.md)             | Configuration guide                            | Developers, DevOps      |
| [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Release process guide                          | Maintainers             |
| [MIGRATION-GUIDE.md](MIGRATION-GUIDE.md)         | Migration guide from Ant to Gradle             | All users               |

## Documentation by Topic

### Build System

| Topic                          | Document                                          | Section                           |
|-------------------------------|--------------------------------------------------|-----------------------------------|
| Overview                      | [README.md](README.md)                           | Build System                      |
| Architecture                  | [README.md](README.md)                           | Build System > Architecture       |
| Build Flow                    | [README.md](README.md)                           | Build System > Build Flow         |

### Tasks

| Topic                          | Document                                          | Section                           |
|-------------------------------|--------------------------------------------------|-----------------------------------|
| All Tasks                     | [TASKS.md](TASKS.md)                             | -                                 |
| Build Tasks                   | [TASKS.md](TASKS.md)                             | Build Tasks                       |
| Verification Tasks            | [TASKS.md](TASKS.md)                             | Verification Tasks                |
| Help Tasks                    | [TASKS.md](TASKS.md)                             | Help Tasks                        |
| Documentation Tasks           | [TASKS.md](TASKS.md)                             | Documentation Tasks               |
| Task Dependencies             | [TASKS.md](TASKS.md)                             | Task Dependencies                 |

### Configuration

| Topic                          | Document                                          | Section                           |
|-------------------------------|--------------------------------------------------|-----------------------------------|
| Configuration Files           | [CONFIGURATION.md](CONFIGURATION.md)             | Configuration Files               |
| Build Properties              | [CONFIGURATION.md](CONFIGURATION.md)             | Build Properties                  |
| Gradle Properties             | [CONFIGURATION.md](CONFIGURATION.md)             | Gradle Properties                 |
| Release Properties            | [CONFIGURATION.md](CONFIGURATION.md)             | Release Properties                |
| Environment Variables         | [CONFIGURATION.md](CONFIGURATION.md)             | Environment Variables             |
| Advanced Configuration        | [CONFIGURATION.md](CONFIGURATION.md)             | Advanced Configuration            |

### Release Process

| Topic                          | Document                                          | Section                           |
|-------------------------------|--------------------------------------------------|-----------------------------------|
| Overview                      | [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Overview                          |
| Prerequisites                 | [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Prerequisites                     |
| Release Workflow              | [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Release Workflow                  |
| Step-by-Step Guide            | [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Step-by-Step Guide                |
| Version Management            | [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Version Management                |
| Publishing Releases           | [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Publishing Releases               |
| Post-Release Tasks            | [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Post-Release Tasks                |

### Troubleshooting

| Topic                          | Document                                          | Section                           |
|-------------------------------|--------------------------------------------------|-----------------------------------|
| Common Issues                 | [README.md](README.md)                           | Troubleshooting                   |
| Configuration Issues          | [CONFIGURATION.md](CONFIGURATION.md)             | Troubleshooting Configuration     |
| Release Issues                | [RELEASE-PROCESS.md](RELEASE-PROCESS.md)         | Troubleshooting                   |

## Common Tasks

### For New Users

1. Read [README.md](README.md) - Overview and quick start
2. Run `gradle info` - Display build information
3. Run `gradle verify` - Verify build environment
4. Run `gradle listVersions` - List available versions

### For Developers

1. Read [TASKS.md](TASKS.md) - Learn available tasks
2. Read [CONFIGURATION.md](CONFIGURATION.md) - Understand configuration
3. Run `gradle tasks` - List all tasks
4. Run `gradle release -PbundleVersion=1.6.39` - Build a release

### For Maintainers

1. Read [RELEASE-PROCESS.md](RELEASE-PROCESS.md) - Learn release process
2. Follow step-by-step guide for releases
3. Update documentation as needed
4. Monitor issues and feedback

## Quick Reference

### Essential Commands

```bash
# Display build information
gradle info

# List available versions
gradle listVersions

# List all releases
gradle listReleases

# Verify environment
gradle verify

# Build release
gradle release -PbundleVersion=1.6.39

# Clean build artifacts
gradle clean
```

### Essential Files

| File                    | Purpose                                  | Documentation                                 |
|------------------------|------------------------------------------|-----------------------------------------------|
| `build.gradle`         | Main build script                        | [README.md](README.md)                        |
| `build.properties`     | Bundle configuration                     | [CONFIGURATION.md](CONFIGURATION.md)          |
| `gradle.properties`    | Gradle settings                          | [CONFIGURATION.md](CONFIGURATION.md)          |
| `releases.properties`  | Release history                          | [CONFIGURATION.md](CONFIGURATION.md)          |
| `settings.gradle`      | Project settings                         | [README.md](README.md)                        |

### Essential Directories

| Directory              | Purpose                                  | Documentation                                 |
|-----------------------|------------------------------------------|-----------------------------------------------|
| `bin/`                | Memcached version binaries               | [README.md](README.md)                        |
| `.gradle-docs/`       | Build documentation                      | This file                                     |
| `build/`              | Gradle build output                      | [README.md](README.md)                        |

## Documentation Standards

### Format

All documentation uses Markdown format with:

- Clear headings and structure
- Tables for reference information
- Code blocks for examples
- Consistent formatting

### Tables

Tables are aligned with consistent column widths:

```markdown
| Column 1             | Column 2             | Column 3             |
|---------------------|---------------------|---------------------|
| Value 1             | Value 2             | Value 3             |
```

### Code Blocks

Code blocks specify the language:

```markdown
```bash
gradle info
```
```

### Links

Internal links use relative paths:

```markdown
[README.md](README.md)
```

External links use full URLs:

```markdown
[Bearsampp](https://bearsampp.com)
```

## Maintenance

### Updating Documentation

When updating documentation:

1. Update relevant document(s)
2. Update this index if structure changes
3. Update version and date at bottom
4. Test all links
5. Verify formatting

### Version Information

All documentation includes version information:

```markdown
---

**Last Updated:** YYYY-MM-DD  
**Version:** YYYY.M.D  
**Maintainer:** Bearsampp Team
```

### Review Schedule

Documentation should be reviewed:

- After each release
- When build system changes
- When new features are added
- Quarterly for accuracy

## Contributing to Documentation

### Guidelines

- Use clear, concise language
- Provide examples
- Keep tables aligned
- Test all commands
- Update index when adding sections

### Style Guide

- Use present tense
- Use active voice
- Use consistent terminology
- Use proper Markdown formatting
- Include code examples

### Submitting Changes

1. Fork the repository
2. Create a documentation branch
3. Make your changes
4. Test all examples
5. Submit a pull request

## Support

### Getting Help

- Read the documentation
- Check troubleshooting sections
- Search GitHub issues
- Ask in discussions

### Reporting Issues

Report documentation issues at:

https://github.com/bearsampp/bearsampp/issues

Include:

- Document name
- Section name
- Issue description
- Suggested improvement

## External Resources

### Gradle

- [Gradle Documentation](https://docs.gradle.org/)
- [Gradle User Manual](https://docs.gradle.org/current/userguide/userguide.html)
- [Gradle Build Language Reference](https://docs.gradle.org/current/dsl/)

### Memcached

- [Memcached Official](https://memcached.org/)
- [Memcached Documentation](https://github.com/memcached/memcached/wiki)
- [Memcached Downloads](https://memcached.org/downloads)

### Bearsampp

- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [Bearsampp Website](https://bearsampp.com)
- [Bearsampp Documentation](https://bearsampp.com/docs)

### Tools

- [7-Zip](https://www.7-zip.org/)
- [Git](https://git-scm.com/)
- [Java](https://adoptium.net/)

## Document History

| Version      | Date         | Changes                                          |
|-------------|--------------|--------------------------------------------------|
| 2025.8.20   | 2025-08-20   | Initial documentation for pure Gradle build      |

---

**Last Updated:** 2025-08-20  
**Version:** 2025.8.20  
**Maintainer:** Bearsampp Team
