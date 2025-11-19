<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-memcached.svg?style=flat-square)](https://github.com/bearsampp/module-memcached/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-memcached/total.svg?style=flat-square)

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving Memcached.

## Build System

This project uses **Gradle** as its build system. The legacy Ant build has been fully replaced with a modern, pure Gradle implementation.

### Key Features

- **Automatic Downloads**: Fetches Memcached versions from [modules-untouched](https://github.com/Bearsampp/modules-untouched)
- **Smart Caching**: Downloads are cached to speed up subsequent builds
- **Multiple Formats**: Supports .7z, .zip, and .tar.gz archives
- **Hash Generation**: Automatically creates MD5, SHA1, SHA256, and SHA512 checksums

### Quick Start

```bash
# Display build information
gradle info

# List available versions from modules-untouched
gradle listReleases

# Build a release (downloads automatically from modules-untouched)
gradle release -PbundleVersion=1.6.29

# Build all versions
gradle releaseAll

# Clean build artifacts
gradle clean
```

**Note**: The build automatically downloads and extracts Memcached versions from the modules-untouched repository. You don't need to manually download or place files in the `bin/` directory.

### Prerequisites

| Requirement       | Version       | Purpose                                  |
|-------------------|---------------|------------------------------------------|
| **Java**          | 8+            | Required for Gradle execution            |
| **Gradle**        | 8.0+          | Build automation tool                    |
| **7-Zip**         | Latest        | Archive creation (.7z format)            |

### Available Tasks

| Task                  | Description                                      |
|-----------------------|--------------------------------------------------|
| `release`             | Build release package (interactive/non-interactive) |
| `releaseAll`          | Build releases for all available versions        |
| `clean`               | Clean build artifacts and temporary files        |
| `verify`              | Verify build environment and dependencies        |
| `info`                | Display build configuration information          |
| `listVersions`        | List available bundle versions in bin/           |
| `listReleases`        | List available releases from modules-untouched   |
| `validateProperties`  | Validate build.properties configuration          |
| `checkModulesUntouched` | Check modules-untouched integration            |

For complete documentation, see [.gradle-docs/README.md](.gradle-docs/README.md)

## Documentation

- **Build Documentation**: [.gradle-docs/README.md](.gradle-docs/README.md)
- **Task Reference**: [.gradle-docs/TASKS.md](.gradle-docs/TASKS.md)
- **Configuration Guide**: [.gradle-docs/CONFIGURATION.md](.gradle-docs/CONFIGURATION.md)
- **Module Downloads**: https://bearsampp.com/module/memcached

## Issues

Issues must be reported on [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).
