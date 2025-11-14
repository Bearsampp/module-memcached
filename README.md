<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-memcached.svg?style=flat-square)](https://github.com/bearsampp/module-memcached/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-memcached/total.svg?style=flat-square)

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving Memcached.

## Overview

This module provides Memcached binaries for the Bearsampp development environment. The build system uses pure Gradle for packaging and releasing different versions of Memcached.

## Quick Start

### Prerequisites

| Requirement          | Version      | Description                                    |
|---------------------|--------------|------------------------------------------------|
| Java                | 8+           | Required for Gradle                            |
| Gradle              | 7.0+         | Build automation tool                          |
| 7-Zip               | Latest       | For creating .7z archives                      |

### Basic Commands

```bash
# Display build information
gradle info

# List available versions
gradle listVersions

# Build a specific version
gradle release -PbundleVersion=1.6.39

# Clean build artifacts
gradle clean

# Verify build environment
gradle verify
```

## Documentation

Comprehensive build documentation is available in the `.gradle-docs/` directory:

| Document                                          | Description                                    |
|--------------------------------------------------|------------------------------------------------|
| [README.md](.gradle-docs/README.md)              | Complete build documentation                   |
| [TASKS.md](.gradle-docs/TASKS.md)                | Detailed task reference                        |
| [CONFIGURATION.md](.gradle-docs/CONFIGURATION.md)| Configuration guide                            |
| [RELEASE-PROCESS.md](.gradle-docs/RELEASE-PROCESS.md) | Release process guide                    |

## Available Tasks

### Build Tasks

| Task                | Description                                    | Example                                      |
|--------------------|------------------------------------------------|----------------------------------------------|
| `release`          | Build release package                          | `gradle release -PbundleVersion=1.6.39`      |
| `clean`            | Clean build artifacts                          | `gradle clean`                               |

### Verification Tasks

| Task                    | Description                                | Example                          |
|------------------------|--------------------------------------------|---------------------------------|
| `verify`               | Verify build environment                   | `gradle verify`                 |
| `validateProperties`   | Validate build.properties                  | `gradle validateProperties`     |

### Help Tasks

| Task              | Description                                    | Example                    |
|------------------|------------------------------------------------|----------------------------|
| `info`           | Display build information                      | `gradle info`              |
| `listVersions`   | List available bundle versions                 | `gradle listVersions`      |
| `listReleases`   | List all releases from releases.properties     | `gradle listReleases`      |
| `tasks`          | List all available tasks                       | `gradle tasks`             |

## Configuration

### build.properties

Main configuration file for the bundle:

```properties
bundle.name     = memcached
bundle.release  = 2025.8.20
bundle.type     = bins
bundle.format   = 7z
```

### Available Versions

The following Memcached versions are available in the `bin/` directory:

- 1.6.15
- 1.6.17
- 1.6.18
- 1.6.21
- 1.6.24
- 1.6.29
- 1.6.31
- 1.6.32
- 1.6.33
- 1.6.36
- 1.6.37
- 1.6.38
- 1.6.39

## Release Process

### Quick Release

```bash
# 1. Update build.properties with release date
# 2. Verify environment
gradle verify

# 3. Build release
gradle release -PbundleVersion=1.6.39

# 4. Test package
7z t C:\Users\troy\Bearsampp-build\release\bearsampp-memcached-1.6.39-2025.8.20.7z

# 5. Create Git tag and push
git tag -a 2025.8.20 -m "Release 2025.8.20"
git push origin main --tags

# 6. Create GitHub release and upload package
# 7. Update releases.properties
```

See [RELEASE-PROCESS.md](.gradle-docs/RELEASE-PROCESS.md) for detailed instructions.

## Downloads

Official releases are available at:

https://bearsampp.com/module/memcached

Or directly from GitHub:

https://github.com/bearsampp/module-memcached/releases

## Issues

Issues must be reported on [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).

## Contributing

### Development Workflow

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the build:
   ```bash
   gradle verify
   gradle release -PbundleVersion=1.6.39
   ```
5. Submit a pull request

### Testing

Before submitting changes:

```bash
# Verify environment
gradle verify

# Validate properties
gradle validateProperties

# Test release build
gradle release -PbundleVersion=1.6.39

# Clean up
gradle clean
```

## License

See [LICENSE](LICENSE) file for details.

## Links

- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [Bearsampp Website](https://bearsampp.com)
- [Memcached Official](https://memcached.org)
- [Build Documentation](.gradle-docs/README.md)

---

**Maintainer:** Bearsampp Team  
**Last Updated:** 2025-08-20
