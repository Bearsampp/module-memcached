# Bearsampp Module Memcached - Gradle Build Documentation

## Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Build Tasks](#build-tasks)
- [Configuration](#configuration)
- [Architecture](#architecture)
- [Troubleshooting](#troubleshooting)
- [Migration Guide](#migration-guide)

---

## Overview

The Bearsampp Module Memcached project has been converted to a **pure Gradle build system**, replacing the legacy Ant build configuration. This provides:

- **Modern Build System**     - Native Gradle tasks and conventions
- **Better Performance**       - Incremental builds and caching
- **Simplified Maintenance**   - Pure Groovy/Gradle DSL
- **Enhanced Tooling**         - IDE integration and dependency management
- **Cross-Platform Support**   - Works on Windows, Linux, and macOS

### Project Information

| Property          | Value                                    |
|-------------------|------------------------------------------|
| **Project Name**  | module-memcached                         |
| **Group**         | com.bearsampp.modules                    |
| **Type**          | Memcached Module Builder                 |
| **Build Tool**    | Gradle 8.x+                              |
| **Language**      | Groovy (Gradle DSL)                      |

---

## Quick Start

### Prerequisites

| Requirement       | Version       | Purpose                                  |
|-------------------|---------------|------------------------------------------|
| **Java**          | 8+            | Required for Gradle execution            |
| **Gradle**        | 8.0+          | Build automation tool                    |
| **7-Zip**         | Latest        | Archive creation (.7z format)            |

### Basic Commands

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

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/bearsampp/module-memcached.git
cd module-memcached
```

### 2. Verify Environment

```bash
gradle verify
```

This will check:
- Java version (8+)
- Required files (build.properties)
- Directory structure (bin/, bin/archived/)
- Build dependencies (7-Zip for .7z format)

### 3. List Available Versions

```bash
gradle listVersions
```

### 4. Build Your First Release

```bash
# Interactive mode (prompts for version)
gradle release

# Or specify version directly
gradle release -PbundleVersion=1.6.29
```

---

## Build Tasks

### Core Build Tasks

| Task                  | Description                                      | Example                                  |
|-----------------------|--------------------------------------------------|------------------------------------------|
| `release`             | Build and package release (interactive/non-interactive) | `gradle release -PbundleVersion=1.6.29` |
| `releaseAll`          | Build releases for all available versions        | `gradle releaseAll`                      |
| `clean`               | Clean build artifacts and temporary files        | `gradle clean`                           |

### Verification Tasks

| Task                      | Description                                  | Example                                      |
|---------------------------|----------------------------------------------|----------------------------------------------|
| `verify`                  | Verify build environment and dependencies    | `gradle verify`                              |
| `validateProperties`      | Validate build.properties configuration      | `gradle validateProperties`                  |
| `checkModulesUntouched`   | Check modules-untouched integration          | `gradle checkModulesUntouched`               |

### Information Tasks

| Task                | Description                                      | Example                    |
|---------------------|--------------------------------------------------|----------------------------|
| `info`              | Display build configuration information          | `gradle info`              |
| `listVersions`      | List available bundle versions in bin/           | `gradle listVersions`      |
| `listReleases`      | List all available releases from modules-untouched | `gradle listReleases`  |

### Task Groups

| Group            | Purpose                                          |
|------------------|--------------------------------------------------|
| **build**        | Build and package tasks                          |
| **verification** | Verification and validation tasks                |
| **help**         | Help and information tasks                       |

---

## Configuration

### build.properties

The main configuration file for the build:

```properties
bundle.name     = memcached
bundle.release  = 2025.8.20
bundle.type     = bins
bundle.format   = 7z
```

| Property          | Description                          | Example Value  |
|-------------------|--------------------------------------|----------------|
| `bundle.name`     | Name of the bundle                   | `memcached`    |
| `bundle.release`  | Release version                      | `2025.8.20`    |
| `bundle.type`     | Type of bundle                       | `bins`         |
| `bundle.format`   | Archive format (7z or zip)           | `7z`           |

### gradle.properties

Gradle-specific configuration:

```properties
# Gradle daemon configuration
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true

# JVM settings
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m
```

### Directory Structure

```
module-memcached/
├── .gradle-docs/          # Gradle documentation
│   ├── README.md          # Main documentation
│   ├── TASKS.md           # Task reference
│   ├── CONFIGURATION.md   # Configuration guide
│   └── MIGRATION.md       # Migration guide
├── bin/                   # Memcached version bundles
│   ├── memcached1.6.29/
│   ├── memcached1.6.39/
│   └── archived/          # Archived versions
│       └── memcached1.5.22/
├── bearsampp-build/       # External build directory (outside repo)
│   ├── tmp/               # Temporary build files
│   │   ├── bundles_prep/bins/memcached/  # Prepared bundles
│   │   └── bundles_build/bins/memcached/ # Build staging
│   └── bins/memcached/    # Final packaged archives
│       └── 2025.8.20/     # Release version
│           ├── bearsampp-memcached-1.6.29-2025.8.20.7z
│           ├── bearsampp-memcached-1.6.29-2025.8.20.7z.md5
│           ├── bearsampp-memcached-1.6.29-2025.8.20.7z.sha1
│           ├── bearsampp-memcached-1.6.29-2025.8.20.7z.sha256
│           └── bearsampp-memcached-1.6.29-2025.8.20.7z.sha512
├── build.gradle           # Main Gradle build script
├── settings.gradle        # Gradle settings
├── build.properties       # Build configuration
└── releases.properties    # Release history
```

---

## Architecture

### Build Process Flow

```
1. User runs: gradle release -PbundleVersion=1.6.29
                    ↓
2. Validate environment and version
                    ↓
3. Create preparation directory (tmp/bundles_prep/)
                    ↓
4. Copy base Memcached files from bin/memcached1.6.29/
                    ↓
5. Output prepared bundle to tmp/bundles_prep/
                    ↓
6. Copy to build staging (tmp/bundles_build/)
                    ↓
7. Package prepared folder into archive
   - Location: bearsampp-build/bins/memcached/{bundle.release}/
   - The archive includes the top-level folder: memcached{version}/
                    ↓
8. Generate hash files (MD5, SHA1, SHA256, SHA512)
```

### Packaging Details

- **Archive name format**: `bearsampp-memcached-{version}-{bundle.release}.{7z|zip}`
- **Location**: `bearsampp-build/bins/memcached/{bundle.release}/`
  - Example: `bearsampp-build/bins/memcached/2025.8.20/bearsampp-memcached-1.6.29-2025.8.20.7z`
- **Content root**: The top-level folder inside the archive is `memcached{version}/` (e.g., `memcached1.6.29/`)
- **Structure**: The archive contains the Memcached version folder at the root with all Memcached files inside

**Archive Structure Example**:
```
bearsampp-memcached-1.6.29-2025.8.20.7z
└── memcached1.6.29/       ← Version folder at root
    ├── memcached.exe
    ├── memcached.conf
    └── ...
```

**Verification Commands**:

```bash
# List archive contents with 7z
7z l bearsampp-build/bins/memcached/2025.8.20/bearsampp-memcached-1.6.29-2025.8.20.7z | more

# You should see entries beginning with:
#   memcached1.6.29/memcached.exe
#   memcached1.6.29/memcached.conf
#   memcached1.6.29/...

# Extract and inspect with PowerShell (zip example)
Expand-Archive -Path bearsampp-build/bins/memcached/2025.8.20/bearsampp-memcached-1.6.29-2025.8.20.zip -DestinationPath .\_inspect
Get-ChildItem .\_inspect\memcached1.6.29 | Select-Object Name

# Expected output:
#   memcached.exe
#   memcached.conf
#   ...
```

**Note**: This archive structure matches the standard Bearsampp module pattern where archives contain `{module}{version}/` at the root. This ensures consistency across all Bearsampp modules.

**Hash Files**: Each archive is accompanied by hash sidecar files:
- `.md5` - MD5 checksum
- `.sha1` - SHA-1 checksum
- `.sha256` - SHA-256 checksum
- `.sha512` - SHA-512 checksum

Example:
```
bearsampp-build/bins/memcached/2025.8.20/
├── bearsampp-memcached-1.6.29-2025.8.20.7z
├── bearsampp-memcached-1.6.29-2025.8.20.7z.md5
├── bearsampp-memcached-1.6.29-2025.8.20.7z.sha1
├── bearsampp-memcached-1.6.29-2025.8.20.7z.sha256
└── bearsampp-memcached-1.6.29-2025.8.20.7z.sha512
```

### Modules-Untouched Integration

The build system integrates with the [modules-untouched repository](https://github.com/Bearsampp/modules-untouched) to fetch version information:

- **Repository**: `https://github.com/Bearsampp/modules-untouched`
- **Properties File**: `modules/memcached.properties`
- **Purpose**: Centralized version management and download URLs

The `listReleases` and `checkModulesUntouched` tasks query this repository to display available Memcached versions.

---

## Troubleshooting

### Common Issues

#### Issue: "Dev path not found"

**Symptom:**
```
Dev path not found: E:/Bearsampp-development/dev
```

**Solution:**
This is a warning only. The dev path is optional for most tasks. If you need it, ensure the `dev` project exists in the parent directory.

---

#### Issue: "Bundle version not found"

**Symptom:**
```
Bundle version not found: E:/Bearsampp-development/module-memcached/bin/memcached1.6.99
```

**Solution:**
1. List available versions: `gradle listVersions`
2. Use an existing version: `gradle release -PbundleVersion=1.6.29`

---

#### Issue: "7-Zip executable not found"

**Symptom:**
```
7-Zip executable not found!
```

**Solution:**
1. Install 7-Zip from https://www.7-zip.org/
2. Or set the `7Z_HOME` environment variable to your 7-Zip installation directory
3. Or use ZIP format instead by changing `bundle.format=zip` in build.properties

---

#### Issue: "Java version too old"

**Symptom:**
```
Java 8+ required
```

**Solution:**
1. Check Java version: `java -version`
2. Install Java 8 or higher
3. Update JAVA_HOME environment variable

---

### Debug Mode

Run Gradle with debug output:

```bash
gradle release -PbundleVersion=1.6.29 --info
gradle release -PbundleVersion=1.6.29 --debug
```

### Clean Build

If you encounter issues, try a clean build:

```bash
gradle clean
gradle release -PbundleVersion=1.6.29
```

---

## Migration Guide

### From Ant to Gradle

The project has been fully migrated from Ant to Gradle. Here's what changed:

#### Removed Files

| File              | Status    | Replacement                |
|-------------------|-----------|----------------------------|
| `build.xml`       | ❌ Removed | `build.gradle`             |

#### Command Mapping

| Ant Command                          | Gradle Command                              |
|--------------------------------------|---------------------------------------------|
| `ant release`                        | `gradle release`                            |
| `ant release -Dinput.bundle=1.6.29`  | `gradle release -PbundleVersion=1.6.29`     |
| `ant clean`                          | `gradle clean`                              |

#### Key Differences

| Aspect              | Ant                          | Gradle                           |
|---------------------|------------------------------|----------------------------------|
| **Build File**      | XML (build.xml)              | Groovy DSL (build.gradle)        |
| **Task Definition** | `<target name="...">`        | `tasks.register('...')`          |
| **Properties**      | `<property name="..." />`    | `ext { ... }`                    |
| **Dependencies**    | Manual downloads             | Automatic with repositories      |
| **Caching**         | None                         | Built-in incremental builds      |
| **IDE Support**     | Limited                      | Excellent (IntelliJ, Eclipse)    |

For complete migration details, see [MIGRATION.md](MIGRATION.md).

---

## Additional Resources

- [Gradle Documentation](https://docs.gradle.org/)
- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [Memcached Official](https://memcached.org/)
- [Memcached Downloads](https://memcached.org/downloads)

---

## Support

For issues and questions:

- **GitHub Issues**: https://github.com/bearsampp/module-memcached/issues
- **Bearsampp Issues**: https://github.com/bearsampp/bearsampp/issues
- **Documentation**: https://bearsampp.com/module/memcached

---

**Last Updated**: 2025-08-20  
**Version**: 2025.8.20  
**Build System**: Pure Gradle (no Ant)

Notes:
- This project deliberately does not ship the Gradle Wrapper. Install Gradle 8+ locally and run with `gradle ...`.
- Legacy Ant files (e.g., Eclipse `.launch` referencing `build.xml`) are deprecated and not used by the build.
