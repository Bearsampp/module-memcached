# Documentation Index

Complete index of all Gradle build documentation for Bearsampp Module Memcached.

---

## Quick Links

| Document              | Description                                      | Link                          |
|-----------------------|--------------------------------------------------|-------------------------------|
| **Main Documentation**| Complete build system guide                      | [README.md](README.md)        |
| **Task Reference**    | All available Gradle tasks                       | [TASKS.md](TASKS.md)          |
| **Configuration**     | Configuration files and properties               | [CONFIGURATION.md](CONFIGURATION.md) |
| **Migration Guide**   | Ant to Gradle migration guide                    | [MIGRATION.md](MIGRATION.md)  |
| **Build System**      | Build system specification                       | [BUILD-SYSTEM.md](BUILD-SYSTEM.md) |
| **Design Decisions**  | Architecture and design decisions                | [DESIGN-DECISIONS.md](DESIGN-DECISIONS.md) |
| **Migration Summary** | Complete migration summary                       | [MIGRATION-SUMMARY.md](MIGRATION-SUMMARY.md) |
| **Docs Update**       | Documentation update summary                     | [DOCS-UPDATE-SUMMARY.md](DOCS-UPDATE-SUMMARY.md) |
| **Final Summary**     | Final project summary                            | [FINAL-SUMMARY.md](FINAL-SUMMARY.md) |

---

## Documentation Structure

```
.gradle-docs/
├── INDEX.md                      # This file - Documentation index
├── README.md                     # Main documentation and quick start
├── TASKS.md                      # Complete task reference
├── CONFIGURATION.md              # Configuration guide
├── MIGRATION.md                  # Ant to Gradle migration guide
├── BUILD-SYSTEM.md               # Build system specification
├── DESIGN-DECISIONS.md           # Architecture and design decisions
├── MIGRATION-SUMMARY.md          # Complete migration summary
├── DOCS-UPDATE-SUMMARY.md        # Documentation update summary
├── FINAL-SUMMARY.md              # Final project summary
└── build.gradle.bruno-reference  # Reference build file
```

---

## Getting Started

### New Users

1. **Start Here**: [README.md](README.md) - Overview and quick start
2. **Verify Setup**: Run `gradle verify` to check environment
3. **List Tasks**: Run `gradle tasks` to see available tasks
4. **Build Release**: Run `gradle release -PbundleVersion=1.6.29`

### Migrating from Ant

1. **Migration Guide**: [MIGRATION.md](MIGRATION.md) - Complete migration guide
2. **Command Mapping**: See command equivalents in migration guide
3. **File Changes**: Understand what changed from Ant to Gradle
4. **Troubleshooting**: Common migration issues and solutions

### Advanced Users

1. **Task Reference**: [TASKS.md](TASKS.md) - All tasks with examples
2. **Configuration**: [CONFIGURATION.md](CONFIGURATION.md) - Advanced configuration
3. **Custom Tasks**: Create custom tasks using Gradle DSL

---

## Documentation by Topic

### Build System

| Topic                 | Document              | Section                          |
|-----------------------|-----------------------|----------------------------------|
| Overview              | README.md             | Overview                         |
| Quick Start           | README.md             | Quick Start                      |
| Installation          | README.md             | Installation                     |
| Architecture          | README.md             | Architecture                     |

### Tasks

| Topic                 | Document              | Section                          |
|-----------------------|-----------------------|----------------------------------|
| Build Tasks           | TASKS.md              | Build Tasks                      |
| Verification Tasks    | TASKS.md              | Verification Tasks               |
| Information Tasks     | TASKS.md              | Information Tasks                |
| Task Examples         | TASKS.md              | Task Examples                    |

### Configuration

| Topic                 | Document              | Section                          |
|-----------------------|-----------------------|----------------------------------|
| Build Properties      | CONFIGURATION.md      | Build Properties                 |
| Gradle Properties     | CONFIGURATION.md      | Gradle Properties                |
| Release Properties    | CONFIGURATION.md      | Release Properties               |
| Environment Variables | CONFIGURATION.md      | Environment Variables            |

### Migration

| Topic                 | Document              | Section                          |
|-----------------------|-----------------------|----------------------------------|
| Overview              | MIGRATION.md          | Overview                         |
| What Changed          | MIGRATION.md          | What Changed                     |
| Command Mapping       | MIGRATION.md          | Command Mapping                  |
| File Changes          | MIGRATION.md          | File Changes                     |
| Troubleshooting       | MIGRATION.md          | Troubleshooting                  |

---

## Common Tasks

### Building

| Task                                      | Document      | Reference                        |
|-------------------------------------------|---------------|----------------------------------|
| Build a release                           | README.md     | Quick Start                      |
| Build specific version                    | TASKS.md      | release task                     |
| Build all versions                        | TASKS.md      | releaseAll task                  |
| Clean build artifacts                     | TASKS.md      | clean task                       |

### Configuration

| Task                                      | Document      | Reference                        |
|-------------------------------------------|---------------|----------------------------------|
| Configure build properties                | CONFIGURATION.md | Build Properties              |
| Configure build paths                     | CONFIGURATION.md | Build Paths                   |
| Configure archive format                  | CONFIGURATION.md | Archive Configuration         |

### Verification

| Task                                      | Document      | Reference                        |
|-------------------------------------------|---------------|----------------------------------|
| Verify build environment                  | TASKS.md      | verify task                      |
| Validate properties                       | TASKS.md      | validateProperties task          |
| Check modules-untouched                   | TASKS.md      | checkModulesUntouched task       |

### Information

| Task                                      | Document      | Reference                        |
|-------------------------------------------|---------------|----------------------------------|
| Display build info                        | TASKS.md      | info task                        |
| List available versions                   | TASKS.md      | listVersions task                |
| List releases                             | TASKS.md      | listReleases task                |

---

## Quick Reference

### Essential Commands

```bash
# Display build information
gradle info

# List all available tasks
gradle tasks

# Verify build environment
gradle verify

# Build a release (interactive)
gradle release

# Build a specific version (non-interactive)
gradle release -PbundleVersion=1.6.29

# Build all versions
gradle releaseAll

# Clean build artifacts
gradle clean
```

### Essential Files

| File                  | Purpose                                  |
|-----------------------|------------------------------------------|
| `build.gradle`        | Main Gradle build script                 |
| `settings.gradle`     | Gradle project settings                  |
| `build.properties`    | Build configuration                      |
| `gradle.properties`   | Gradle-specific settings                 |
| `releases.properties` | Release history                          |

### Essential Directories

| Directory             | Purpose                                  |
|-----------------------|------------------------------------------|
| `bin/`                | Memcached version bundles                |
| `bin/archived/`       | Archived Memcached versions              |
| `bearsampp-build/tmp/` | Temporary build files (external)       |
| `bearsampp-build/bins/` | Final packaged archives (external)    |
| `.gradle-docs/`       | Gradle documentation                     |

---

## Search by Keyword

### A-C

| Keyword               | Document              | Section                          |
|-----------------------|-----------------------|----------------------------------|
| Archive               | CONFIGURATION.md      | Archive Configuration            |
| Architecture          | README.md             | Architecture                     |
| Build                 | TASKS.md              | Build Tasks                      |
| Clean                 | TASKS.md              | clean task                       |
| Configuration         | CONFIGURATION.md      | All sections                     |

### D-M

| Keyword               | Document              | Section                          |
|-----------------------|-----------------------|----------------------------------|
| Dependencies          | README.md             | Installation                     |
| Gradle                | README.md             | All sections                     |
| Hash Files            | README.md             | Architecture                     |
| Info                  | TASKS.md              | info task                        |
| Installation          | README.md             | Installation                     |
| Migration             | MIGRATION.md          | All sections                     |
| Modules-Untouched     | TASKS.md              | checkModulesUntouched task       |

### P-Z

| Keyword               | Document              | Section                          |
|-----------------------|-----------------------|----------------------------------|
| Properties            | CONFIGURATION.md      | Build Properties                 |
| Release               | TASKS.md              | release task                     |
| Tasks                 | TASKS.md              | All sections                     |
| Troubleshooting       | README.md, MIGRATION.md | Troubleshooting sections       |
| Validation            | TASKS.md              | Verification Tasks               |
| Verify                | TASKS.md              | verify task                      |
| Versions              | TASKS.md              | listVersions task                |

---

## Document Summaries

### README.md

**Purpose**: Main documentation and quick start guide

**Contents**:
- Overview of the Gradle build system
- Quick start guide with basic commands
- Installation instructions
- Complete task reference
- Configuration overview
- Architecture and build process flow
- Troubleshooting common issues
- Migration guide summary

**Target Audience**: All users, especially new users

---

### TASKS.md

**Purpose**: Complete reference for all Gradle tasks

**Contents**:
- Build tasks (release, releaseAll, clean)
- Verification tasks (verify, validateProperties, checkModulesUntouched)
- Information tasks (info, listVersions, listReleases)
- Task dependencies and execution order
- Task examples and usage patterns
- Task options and properties

**Target Audience**: Developers and build engineers

---

### CONFIGURATION.md

**Purpose**: Configuration guide for build system

**Contents**:
- Configuration files overview
- Build properties reference
- Gradle properties reference
- Release properties reference
- Environment variables
- Build paths configuration
- Archive format configuration
- Configuration examples
- Best practices

**Target Audience**: Build engineers and advanced users

---

### MIGRATION.md

**Purpose**: Guide for migrating from Ant to Gradle

**Contents**:
- Migration overview
- What changed from Ant to Gradle
- Command mapping (Ant to Gradle)
- File changes
- Configuration changes
- Task equivalents
- Troubleshooting migration issues
- Benefits of migration
- Next steps for developers, CI/CD, and contributors

**Target Audience**: Users migrating from Ant build system

---

## Version History

| Version       | Date       | Changes                                  |
|---------------|------------|------------------------------------------|
| 2025.8.20     | 2025-08-20 | Initial Gradle documentation             |
|               |            | - Created README.md                      |
|               |            | - Created TASKS.md                       |
|               |            | - Created CONFIGURATION.md               |
|               |            | - Created MIGRATION.md                   |
|               |            | - Created INDEX.md                       |

---

## Contributing

To contribute to the documentation:

1. **Fork Repository**: Fork the module-memcached repository
2. **Edit Documentation**: Make changes to documentation files
3. **Follow Style**: Maintain consistent formatting and style
4. **Test Examples**: Verify all code examples work
5. **Submit PR**: Create pull request with changes

### Documentation Style Guide

- Use Markdown formatting
- Include code examples
- Use tables for structured data
- Add links to related sections
- Keep language clear and concise
- Include practical examples

---

## Support

For documentation issues or questions:

- **GitHub Issues**: https://github.com/bearsampp/module-memcached/issues
- **Bearsampp Issues**: https://github.com/bearsampp/bearsampp/issues
- **Documentation**: This directory (.gradle-docs/)

---

**Last Updated**: 2025-08-20  
**Version**: 2025.8.20  
**Total Documents**: 10
