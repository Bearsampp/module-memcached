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
- **Automated Workflow**       - Automatic upstream release creation
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

### Information Tasks

| Task                | Description                                      | Example                    |
|---------------------|--------------------------------------------------|----------------------------|
| `info`              | Display build configuration information          | `gradle info`              |
| `listVersions`      | List available bundle versions in bin/           | `gradle listVersions`      |

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

### Automated Upstream Workflow

The build system includes an **intelligent automated workflow** that handles upstream releases:

#### Smart Version Detection

When you run `gradle release -PbundleVersion=1.6.39`, the build automatically:

1. **Checks modules-untouched** - Does version 1.6.39 exist?
   - ✅ **YES** → Skip to step 6 (normal build process)
   - ❌ **NO** → Continue to step 2 (create upstream release)

2. **Downloads from nono303** - Clones nono303/memcached repository and extracts `libevent-2.1/x64` folder

3. **Extracts version** - Uses PowerShell to get version from `memcached.exe` file properties

4. **Creates 7z archive** - Creates `memcached-{version}-win64.7z` with all binaries

5. **Creates GitHub release**:
   - Tag: `memcached-{bundle.release}` (e.g., `memcached-2025.8.20`)
   - Title: `Memcache {VERSION}` (e.g., `Memcache 1.6.39`)
   - Description: `Memcached {VERSION} automated build`
   - Asset: The 7z file
   - Uses GitHub CLI (`gh`) with `GH_PAT` environment variable

6. **Waits for update** - Polls modules-untouched every 10 seconds (max 5 minutes) until version appears

7. **Normal build process**:
   - Downloads binaries from modules-untouched
   - Overlays configuration from `bin/memcached{version}/`
   - Packages final release
   - Generates hash files

#### Prerequisites for Upstream Workflow

| Requirement       | Purpose                                          |
|-------------------|--------------------------------------------------|
| **GitHub CLI**    | Creating releases (`gh release create`)          |
| **GH_PAT**        | GitHub Personal Access Token (environment var)   |
| **7-Zip**         | Creating 7z archives                             |

Set up GitHub authentication:
```bash
# Set environment variable
$env:GH_PAT = 'your_github_personal_access_token'

# Or authenticate with gh CLI
gh auth login
```

#### Example Workflow

```bash
# Build a new version that doesn't exist in modules-untouched yet
gradle release -PbundleVersion=1.6.40

# Output:
# ======================================================================
# Building memcached 1.6.40
# ======================================================================
#
# Checking modules-untouched for version 1.6.40...
# ⚠ Version 1.6.40 not found in modules-untouched
# Starting upstream release process...
#
# ======================================================================
# Downloading Memcached 1.6.40 from nono303/memcached
# ======================================================================
# Repository: https://github.com/nono303/memcached
# Branch: master
# Subfolder: libevent-2.1/x64
# ...
# Successfully copied 15 files
#
# ======================================================================
# Creating Upstream Release
# ======================================================================
# Memcached version: 1.6.40
# Creating archive: memcached-1.6.40-win64.7z
# ...
# Creating GitHub release:
#   Tag: memcached-2025.8.20
#   Title: Memcache 1.6.40
#   Asset: memcached-1.6.40-win64.7z
# GitHub release created successfully!
#
# ======================================================================
# Waiting for modules-untouched to be updated...
# ======================================================================
# [1] Checking modules-untouched (0s elapsed)...
#   Not yet available, waiting 10s...
# [2] Checking modules-untouched (10s elapsed)...
# ✓ Version 1.6.40 found in modules-untouched!
#
# ======================================================================
# Continuing with normal release process
# ======================================================================
# ...
```

### Version Management

The build system uses a **two-source approach** for building releases:

1. **Binaries**: Downloaded automatically from [modules-untouched](https://github.com/Bearsampp/modules-untouched)
2. **Configuration**: Stored locally in `bin/` directory

#### How It Works

When you run `gradle release -PbundleVersion=1.6.39`, the build:

1. **Checks** if version exists in modules-untouched (if not, creates upstream release automatically)
2. **Downloads** Memcached binaries from modules-untouched (cached for reuse)
3. **Extracts** the binaries to a temporary location
4. **Locates** configuration files in `bin/memcached1.6.39/` or `bin/archived/memcached1.6.39/`
5. **Combines** binaries + configuration files
6. **Packages** the release archive
7. **Generates** hash files (MD5, SHA1, SHA256, SHA512)

#### Directory Structure

```
bin/
├── memcached1.6.29/       # Configuration only
│   └── bearsampp.conf
├── memcached1.6.39/       # Configuration only
│   └── bearsampp.conf
└── archived/              # Older version configs
    └── memcached1.5.22/
        └── bearsampp.conf

bearsampp-build/tmp/bundles_src/memcached/  # Downloaded binaries (cached)
├── memcached-1.6.39-win64.7z
└── memcached1.6.39/
    ├── memcached.exe      # From modules-untouched
    ├── memcached-debug.exe
    └── ...
```

#### Verification Tasks

| Task                      | Description                                  |
|---------------------------|----------------------------------------------|
| `listVersions`            | List all versions available in bin/ and bin/archived/ |
| `checkModulesUntouched`   | Check modules-untouched integration and available versions |

Example:
```bash
# See what configuration versions are available locally
gradle listVersions

# Check what binaries are available in modules-untouched
gradle checkModulesUntouched

# Build a specific version (downloads binaries automatically)
gradle release -PbundleVersion=1.6.39
```

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
