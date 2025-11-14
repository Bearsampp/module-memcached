# Bearsampp Module Memcached - Gradle Build Documentation

<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="../img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-memcached.svg?style=flat-square)](https://github.com/bearsampp/module-memcached/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-memcached/total.svg?style=flat-square)

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving Memcached.

## Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [Build System](#build-system)
- [Available Tasks](#available-tasks)
- [Configuration](#configuration)
- [Release Process](#release-process)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## Overview

This module provides Memcached binaries for the Bearsampp development environment. The build system uses pure Gradle for packaging and releasing different versions of Memcached.

### Key Features

- **Pure Gradle Build**     - Modern, maintainable build system (Groovy DSL)
- **No Wrapper Required**   - Uses system-installed Gradle
- **Version Management**    - Easy management of multiple Memcached versions
- **Automated Packaging**   - Automated 7z archive creation
- **Build Verification**    - Comprehensive environment checks
- **Documentation**         - Complete build documentation

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

## Build System

### Architecture

The build system is organized as follows:

```
module-memcached/
├── bin/                          # Memcached version binaries
│   ├── memcached1.6.15/
│   ├── memcached1.6.39/
│   └── ...
├── .gradle-docs/                 # Build documentation
│   ├── README.md
│   ├── TASKS.md
│   ├── CONFIGURATION.md
│   └── RELEASE-PROCESS.md
├── build.gradle                  # Main build script
├── settings.gradle               # Gradle settings
├── build.properties              # Bundle configuration
├── releases.properties           # Release history
└── gradle.properties             # Gradle properties
```

### Build Flow

```
┌─────────────────┐
│  User Command   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Load Config    │ ← build.properties
└────────┬────────┘   releases.properties
         │
         ▼
┌─────────────────┐
│  Verify Env     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Prepare Files  │ ← bin/memcached{version}/
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Create Archive │ → release/bearsampp-memcached-{version}-{date}.7z
└─────────────────┘
```

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

### Documentation Tasks

| Task              | Description                                    | Example                    |
|------------------|------------------------------------------------|----------------------------|
| `generateDocs`   | Generate build documentation                   | `gradle generateDocs`      |

## Configuration

### build.properties

Main configuration file for the bundle:

```properties
bundle.name     = memcached
bundle.release  = 2025.8.20
bundle.type     = bins
bundle.format   = 7z
```

| Property          | Description                      | Default        |
|------------------|----------------------------------|----------------|
| `bundle.name`    | Bundle name                      | `memcached`    |
| `bundle.release` | Release date                     | `2025.8.20`    |
| `bundle.type`    | Bundle type                      | `bins`         |
| `bundle.format`  | Archive format                   | `7z`           |

### gradle.properties

Gradle-specific configuration:

```properties
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.configureondemand=true
```

### releases.properties

Historical release information:

```properties
1.6.39 = https://github.com/Bearsampp/module-memcached/releases/download/2025.8.20/bearsampp-memcached-1.6.39-2025.8.20.7z
```

## Release Process

### Step-by-Step Guide

#### 1. Prepare Version

Ensure the version exists in `bin/` directory:

```bash
bin/
└── memcached1.6.39/
    ├── memcached.exe
    └── ...
```

#### 2. Update Configuration

Update `build.properties` with the release date:

```properties
bundle.release = 2025.8.20
```

#### 3. Verify Environment

```bash
gradle verify
```

Expected output:

```
╔════════════════════════════════════════════════════════════════════════════╗
║                    Build Environment Verification                          ║
╚════════════════════════════════════════════════════════════════════════════╝

Environment Check Results:
────────────────────────────────────────────────────────────────────────────
  ✓ PASS     Java 8+
  ✓ PASS     build.gradle
  ✓ PASS     build.properties
  ✓ PASS     releases.properties
  ✓ PASS     settings.gradle
  ✓ PASS     bin/ directory
  ✓ PASS     7z command
────────────────────────────────────────────────────────────────────────────

[SUCCESS] All checks passed! Build environment is ready.
```

#### 4. Build Release

```bash
gradle release -PbundleVersion=1.6.39
```

Expected output:

```
╔════════════════════════════════════════════════════════════════════════════╗
║                         Release Build                                      ║
╚════════════════════════════════════════════════════════════════════════════╝

Building release for memcached version 1.6.39...

Bundle path: E:/Bearsampp-development/module-memcached/bin/memcached1.6.39

Preparing bundle...
Copying bundle files...
Creating archive: bearsampp-memcached-1.6.39-2025.8.20.7z

╔════════════════════════════════════════════════════════════════════════════╗
║                      Release Build Completed                               ║
╚════════════════════════════════════════════════════════════════════════════╝

Release package created:
  C:\Users\troy\Bearsampp-build\release\bearsampp-memcached-1.6.39-2025.8.20.7z

Package size: 0.45 MB
```

#### 5. Update releases.properties

Add the new release entry:

```properties
1.6.39 = https://github.com/Bearsampp/module-memcached/releases/download/2025.8.20/bearsampp-memcached-1.6.39-2025.8.20.7z
```

#### 6. Commit and Tag

```bash
git add .
git commit -m "Release memcached 1.6.39"
git tag -a 2025.8.20 -m "Release 2025.8.20"
git push origin main --tags
```

## Troubleshooting

### Common Issues

#### Issue: 7z command not found

**Solution:**

Install 7-Zip and add it to your PATH:

```bash
# Windows
set PATH=%PATH%;C:\Program Files\7-Zip
```

#### Issue: Bundle version not found

**Error:**

```
Bundle version not found: E:/Bearsampp-development/module-memcached/bin/memcached1.6.39
```

**Solution:**

1. Check available versions:
   ```bash
   gradle listVersions
   ```

2. Ensure the version directory exists in `bin/`

#### Issue: Java version too old

**Error:**

```
✗ FAIL     Java 8+
```

**Solution:**

Update Java to version 8 or higher:

```bash
java -version
```

### Debug Mode

Run Gradle with debug output:

```bash
gradle release -PbundleVersion=1.6.39 --info
gradle release -PbundleVersion=1.6.39 --debug
```

### Clean Build

If you encounter issues, try a clean build:

```bash
gradle clean
gradle release -PbundleVersion=1.6.39
```

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

### Code Style

- Use 4 spaces for indentation
- Follow Gradle best practices
- Add comments for complex logic
- Update documentation for new features

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

## Additional Resources

### Documentation

- [Tasks Reference](TASKS.md)           - Detailed task documentation
- [Configuration Guide](CONFIGURATION.md) - Configuration options
- [Release Process](RELEASE-PROCESS.md) - Detailed release guide

### External Links

- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [Bearsampp Website](https://bearsampp.com)
- [Memcached Official](https://memcached.org)
- [Gradle Documentation](https://docs.gradle.org)

### Support

- **Issues:** [GitHub Issues](https://github.com/bearsampp/bearsampp/issues)
- **Website:** [bearsampp.com](https://bearsampp.com)
- **Documentation:** [bearsampp.com/module/memcached](https://bearsampp.com/module/memcached)

---

**Last Updated:** 2025-08-20  
**Version:** 2025.8.20  
**Maintainer:** Bearsampp Team
