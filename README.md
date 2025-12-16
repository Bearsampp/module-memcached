<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-memcached.svg?style=flat-square)](https://github.com/bearsampp/module-memcached/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-memcached/total.svg?style=flat-square)

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving Memcached.

## Build System

This project uses **Gradle** as its build system for building and packaging Memcached module releases.

### Key Features

- **Automated Upstream Workflow**: Automatically creates upstream releases if version doesn't exist
- **Smart Version Detection**: Checks modules-untouched and creates releases as needed
- **Automatic Downloads**: Fetches Memcached binaries from [modules-untouched](https://github.com/Bearsampp/modules-untouched)
- **Local Configuration**: Bearsampp-specific configuration files stored in `bin/` directory
- **Smart Caching**: Downloads are cached to speed up subsequent builds
- **Multiple Formats**: Supports .7z and .zip archive formats
- **Hash Generation**: Automatically creates MD5, SHA1, SHA256, and SHA512 checksums
- **Batch Processing**: Build all versions at once with `releaseAll`

### Quick Start

```bash
# Display build information
gradle info

# List available versions in bin/
gradle listVersions

# Build a specific release
gradle release -PbundleVersion=1.6.39

# Build all versions
gradle releaseAll

# Clean build artifacts
gradle clean

# Verify build environment
gradle verify
```

### Prerequisites

| Requirement       | Version       | Purpose                                  |
|-------------------|---------------|------------------------------------------|
| **Java**          | 8+            | Required for Gradle execution            |
| **Gradle**        | 8.0+          | Build automation tool (install locally)  |
| **7-Zip**         | Latest        | Archive creation (.7z format)            |

**Note**: This project does not use Gradle wrapper. Install Gradle 8.0+ locally and run commands with `gradle`.

### Available Tasks

| Task                  | Description                                      |
|-----------------------|--------------------------------------------------|
| `release`             | Build release package (interactive/non-interactive) |
| `releaseAll`          | Build releases for all available versions        |
| `clean`               | Clean build artifacts and temporary files        |
| `verify`              | Verify build environment and dependencies        |
| `info`                | Display build configuration information          |
| `listVersions`        | List available bundle versions in bin/           |

For complete documentation, see [.gradle-docs/README.md](.gradle-docs/README.md)

## Documentation

- **Build Documentation**: [.gradle-docs/README.md](.gradle-docs/README.md)
- **Task Reference**: [.gradle-docs/TASKS.md](.gradle-docs/TASKS.md)
- **Configuration Guide**: [.gradle-docs/CONFIGURATION.md](.gradle-docs/CONFIGURATION.md)
- **Module Downloads**: https://bearsampp.com/module/memcached

## Issues

Issues must be reported on [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).
