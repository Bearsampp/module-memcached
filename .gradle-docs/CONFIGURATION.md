# Configuration Guide

Complete guide for configuring the Bearsampp Module Memcached build system.

## Table of Contents

- [Configuration Files](#configuration-files)
- [Build Properties](#build-properties)
- [Gradle Properties](#gradle-properties)
- [Release Properties](#release-properties)
- [Environment Variables](#environment-variables)
- [Advanced Configuration](#advanced-configuration)

## Configuration Files

### Overview

The build system uses three main configuration files:

| File                    | Purpose                                  | Format       | Required |
|------------------------|------------------------------------------|--------------|----------|
| `build.properties`     | Bundle configuration                     | Properties   | Yes      |
| `gradle.properties`    | Gradle build settings                    | Properties   | Yes      |
| `releases.properties`  | Release history and URLs                 | Properties   | Yes      |
| `settings.gradle`      | Gradle project settings                  | Groovy       | Yes      |

### File Locations

```
module-memcached/
├── build.properties          # Bundle configuration
├── gradle.properties         # Gradle settings
├── releases.properties       # Release history
└── settings.gradle           # Project settings
```

---

## Build Properties

### File: build.properties

Main configuration file for bundle settings.

**Location:** `E:/Bearsampp-development/module-memcached/build.properties`

**Format:** Java Properties

### Properties Reference

#### bundle.name

Bundle name identifier.

| Property          | Type     | Required | Default      | Example      |
|------------------|----------|----------|--------------|--------------|
| `bundle.name`    | String   | Yes      | `memcached`  | `memcached`  |

**Description:** Identifies the bundle name used in file naming and directory structure.

**Usage:**
```properties
bundle.name = memcached
```

**Impact:**
- Used in archive naming: `bearsampp-${bundle.name}-${version}-${release}.7z`
- Used in directory naming: `bin/${bundle.name}${version}/`
- Used in display output

---

#### bundle.release

Release date identifier.

| Property          | Type     | Required | Default      | Example      |
|------------------|----------|----------|--------------|--------------|
| `bundle.release` | String   | Yes      | -            | `2025.8.20`  |

**Description:** Release date in YYYY.M.D format.

**Usage:**
```properties
bundle.release = 2025.8.20
```

**Format:** `YYYY.M.D` or `YYYY.MM.DD`

**Examples:**
- `2025.8.20` - August 20, 2025
- `2025.12.1` - December 1, 2025
- `2024.3.30` - March 30, 2024

**Impact:**
- Used in archive naming
- Used in release URLs
- Used in version tracking

---

#### bundle.type

Bundle type classification.

| Property          | Type     | Required | Default      | Example      |
|------------------|----------|----------|--------------|--------------|
| `bundle.type`    | String   | Yes      | `bins`       | `bins`       |

**Description:** Classifies the bundle type.

**Usage:**
```properties
bundle.type = bins
```

**Valid Values:**
- `bins` - Binary distribution
- `apps` - Application distribution
- `tools` - Tool distribution

**Impact:**
- Used for categorization
- May affect build process in future versions

---

#### bundle.format

Archive format for release packages.

| Property          | Type     | Required | Default      | Example      |
|------------------|----------|----------|--------------|--------------|
| `bundle.format`  | String   | Yes      | `7z`         | `7z`         |

**Description:** Specifies the archive format for release packages.

**Usage:**
```properties
bundle.format = 7z
```

**Valid Values:**
- `7z` - 7-Zip format (recommended)
- `zip` - ZIP format

**Impact:**
- Determines compression tool used
- Affects archive file extension
- Impacts compression ratio

**Requirements:**
- `7z` format requires 7-Zip installed and in PATH
- `zip` format uses Gradle's built-in zip task

---

#### build.path (Optional)

Custom build output directory.

| Property          | Type     | Required | Default                              | Example                    |
|------------------|----------|----------|--------------------------------------|----------------------------|
| `build.path`     | String   | No       | `${user.home}/Bearsampp-build`       | `C:/Bearsampp-build`       |

**Description:** Specifies custom build output directory.

**Usage:**
```properties
build.path = C:/Bearsampp-build
```

**Default Behavior:**
If not specified, uses `${user.home}/Bearsampp-build`

**Directory Structure:**
```
${build.path}/
├── tmp/                      # Temporary build files
│   └── memcached1.6.39/      # Prepared bundle
└── release/                  # Release archives
    └── bearsampp-memcached-1.6.39-2025.8.20.7z
```

---

### Complete Example

```properties
# Bundle Configuration
bundle.name = memcached
bundle.release = 2025.8.20
bundle.type = bins
bundle.format = 7z

# Optional: Custom build path
#build.path = C:/Bearsampp-build
```

---

## Gradle Properties

### File: gradle.properties

Gradle build system configuration.

**Location:** `E:/Bearsampp-development/module-memcached/gradle.properties`

**Format:** Java Properties

### Properties Reference

#### org.gradle.daemon

Enable Gradle daemon for faster builds.

| Property                  | Type      | Required | Default      | Example      |
|--------------------------|-----------|----------|--------------|--------------|
| `org.gradle.daemon`      | Boolean   | No       | `true`       | `true`       |

**Description:** Enables Gradle daemon for improved build performance.

**Usage:**
```properties
org.gradle.daemon=true
```

**Benefits:**
- Faster build times
- Reduced startup overhead
- Improved incremental builds

---

#### org.gradle.parallel

Enable parallel task execution.

| Property                  | Type      | Required | Default      | Example      |
|--------------------------|-----------|----------|--------------|--------------|
| `org.gradle.parallel`    | Boolean   | No       | `true`       | `true`       |

**Description:** Enables parallel execution of independent tasks.

**Usage:**
```properties
org.gradle.parallel=true
```

**Benefits:**
- Faster builds on multi-core systems
- Better resource utilization

---

#### org.gradle.caching

Enable build caching.

| Property                  | Type      | Required | Default      | Example      |
|--------------------------|-----------|----------|--------------|--------------|
| `org.gradle.caching`     | Boolean   | No       | `true`       | `true`       |

**Description:** Enables Gradle build cache for faster incremental builds.

**Usage:**
```properties
org.gradle.caching=true
```

**Benefits:**
- Reuses outputs from previous builds
- Significantly faster incremental builds
- Shared cache across projects

---

#### org.gradle.configureondemand

Enable configuration on demand.

| Property                          | Type      | Required | Default      | Example      |
|----------------------------------|-----------|----------|--------------|--------------|
| `org.gradle.configureondemand`   | Boolean   | No       | `true`       | `true`       |

**Description:** Configures only necessary projects.

**Usage:**
```properties
org.gradle.configureondemand=true
```

**Benefits:**
- Faster configuration phase
- Reduced memory usage

---

#### org.gradle.jvmargs

JVM arguments for Gradle.

| Property                  | Type      | Required | Default      | Example                    |
|--------------------------|-----------|----------|--------------|----------------------------|
| `org.gradle.jvmargs`     | String    | No       | -            | `-Xmx2048m -Xms512m`       |

**Description:** Specifies JVM arguments for Gradle daemon.

**Usage:**
```properties
org.gradle.jvmargs=-Xmx2048m -Xms512m -XX:MaxMetaspaceSize=512m
```

**Common Arguments:**
- `-Xmx2048m` - Maximum heap size (2GB)
- `-Xms512m` - Initial heap size (512MB)
- `-XX:MaxMetaspaceSize=512m` - Maximum metaspace size

---

### Complete Example

```properties
# Gradle Build Configuration

# Enable daemon for faster builds
org.gradle.daemon=true

# Enable parallel execution
org.gradle.parallel=true

# Enable build caching
org.gradle.caching=true

# Enable configuration on demand
org.gradle.configureondemand=true

# JVM arguments
org.gradle.jvmargs=-Xmx2048m -Xms512m -XX:MaxMetaspaceSize=512m

# Console output
org.gradle.console=rich

# Warning mode
org.gradle.warning.mode=all
```

---

## Release Properties

### File: releases.properties

Historical release information and download URLs.

**Location:** `E:/Bearsampp-development/module-memcached/releases.properties`

**Format:** Java Properties

### Format

```properties
<version> = <download_url>
```

### Properties Reference

Each entry represents a released version:

| Key          | Value                                    | Description                           |
|-------------|------------------------------------------|---------------------------------------|
| Version     | Download URL                             | GitHub release download link          |

### Example Entries

```properties
1.6.39 = https://github.com/Bearsampp/module-memcached/releases/download/2025.8.20/bearsampp-memcached-1.6.39-2025.8.20.7z
1.6.38 = https://github.com/Bearsampp/module-memcached/releases/download/2025.4.19/bearsampp-memcached-1.6.38-2025.4.19.7z
1.6.36 = https://github.com/Bearsampp/module-memcached/releases/download/2025.2.11/bearsampp-memcached-1.6.36-2025.2.11.7z
```

### URL Format

```
https://github.com/Bearsampp/module-memcached/releases/download/{release_date}/bearsampp-memcached-{version}-{release_date}.7z
```

**Components:**
- `{release_date}` - Release date from `bundle.release`
- `{version}` - Memcached version number
- `.7z` - Archive extension

### Adding New Releases

When creating a new release:

1. Build the release package
2. Upload to GitHub releases
3. Add entry to `releases.properties`:

```properties
1.6.40 = https://github.com/Bearsampp/module-memcached/releases/download/2025.9.15/bearsampp-memcached-1.6.40-2025.9.15.7z
```

### Sorting

Entries should be sorted by version number (ascending):

```properties
1.6.15 = ...
1.6.17 = ...
1.6.18 = ...
1.6.39 = ...
```

---

## Environment Variables

### Required Variables

#### JAVA_HOME

Java installation directory.

| Variable      | Required | Description                           | Example                                |
|--------------|----------|---------------------------------------|----------------------------------------|
| `JAVA_HOME`  | Yes      | Java JDK installation directory       | `C:\Program Files\Java\jdk-17.0.2`     |

**Usage:**
```bash
# Windows
set JAVA_HOME=C:\Program Files\Java\jdk-17.0.2

# PowerShell
$env:JAVA_HOME="C:\Program Files\Java\jdk-17.0.2"
```

**Verification:**
```bash
echo %JAVA_HOME%
java -version
```

---

#### PATH

System path including required tools.

| Variable      | Required | Description                           | Example                                |
|--------------|----------|---------------------------------------|----------------------------------------|
| `PATH`       | Yes      | System path with 7-Zip                | `C:\Program Files\7-Zip;...`           |

**Required in PATH:**
- 7-Zip (for 7z format)
- Java (for Gradle)
- Gradle (optional, can use wrapper)

**Usage:**
```bash
# Windows
set PATH=%PATH%;C:\Program Files\7-Zip

# PowerShell
$env:PATH="$env:PATH;C:\Program Files\7-Zip"
```

**Verification:**
```bash
7z
gradle --version
```

---

### Optional Variables

#### GRADLE_HOME

Gradle installation directory.

| Variable        | Required | Description                           | Example                                |
|----------------|----------|---------------------------------------|----------------------------------------|
| `GRADLE_HOME`  | No       | Gradle installation directory         | `C:\Gradle\gradle-8.5`                 |

**Usage:**
```bash
# Windows
set GRADLE_HOME=C:\Gradle\gradle-8.5
set PATH=%PATH%;%GRADLE_HOME%\bin
```

---

#### GRADLE_USER_HOME

Gradle user home directory.

| Variable              | Required | Description                           | Example                                |
|----------------------|----------|---------------------------------------|----------------------------------------|
| `GRADLE_USER_HOME`   | No       | Gradle cache and config directory     | `C:\Users\troy\.gradle`                |

**Default:** `${user.home}/.gradle`

**Usage:**
```bash
# Windows
set GRADLE_USER_HOME=C:\Users\troy\.gradle
```

---

## Advanced Configuration

### Custom Build Paths

Override default build paths:

```properties
# build.properties
build.path = D:/CustomBuild
```

**Directory Structure:**
```
D:/CustomBuild/
├── tmp/
│   └── memcached1.6.39/
└── release/
    └── bearsampp-memcached-1.6.39-2025.8.20.7z
```

---

### Performance Tuning

Optimize Gradle performance:

```properties
# gradle.properties

# Increase heap size for large builds
org.gradle.jvmargs=-Xmx4096m -Xms1024m

# Enable all performance features
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.configureondemand=true

# File system watching (Gradle 7.0+)
org.gradle.vfs.watch=true
```

---

### Network Configuration

Configure network settings:

```properties
# gradle.properties

# Proxy settings
systemProp.http.proxyHost=proxy.company.com
systemProp.http.proxyPort=8080
systemProp.https.proxyHost=proxy.company.com
systemProp.https.proxyPort=8080

# Proxy authentication
systemProp.http.proxyUser=username
systemProp.http.proxyPassword=password
```

---

### Logging Configuration

Configure logging levels:

```properties
# gradle.properties

# Console output
org.gradle.console=rich

# Logging level
org.gradle.logging.level=lifecycle

# Warning mode
org.gradle.warning.mode=all
```

**Console Options:**
- `auto` - Automatic detection
- `plain` - Plain text output
- `rich` - Rich console output (default)
- `verbose` - Verbose output

**Warning Modes:**
- `all` - Show all warnings
- `fail` - Fail on warnings
- `summary` - Show warning summary
- `none` - Suppress warnings

---

### Build Cache Configuration

Configure build cache:

```groovy
// settings.gradle

buildCache {
    local {
        enabled = true
        directory = file("${rootDir}/.gradle/build-cache")
        removeUnusedEntriesAfterDays = 30
    }
    
    remote(HttpBuildCache) {
        enabled = false
        url = 'https://cache.example.com/'
        push = false
    }
}
```

---

### Multi-Project Configuration

For multi-project builds:

```groovy
// settings.gradle

rootProject.name = 'module-memcached'

// Include subprojects
// include 'subproject1'
// include 'subproject2'

// Enable features
enableFeaturePreview('STABLE_CONFIGURATION_CACHE')
enableFeaturePreview('TYPESAFE_PROJECT_ACCESSORS')
```

---

## Configuration Validation

### Validate Configuration

```bash
# Validate build.properties
gradle validateProperties

# Verify environment
gradle verify

# Display configuration
gradle info
```

### Configuration Checklist

- [ ] `build.properties` exists and is valid
- [ ] `gradle.properties` exists and is configured
- [ ] `releases.properties` exists and is up-to-date
- [ ] `settings.gradle` exists
- [ ] `JAVA_HOME` is set correctly
- [ ] 7-Zip is in PATH
- [ ] Gradle is installed or wrapper is present
- [ ] `bin/` directory contains version folders

---

## Troubleshooting Configuration

### Common Issues

#### Issue: Property not found

**Error:**
```
Property 'bundle.name' not found
```

**Solution:**
Check `build.properties` contains all required properties:
```properties
bundle.name = memcached
bundle.release = 2025.8.20
bundle.type = bins
bundle.format = 7z
```

---

#### Issue: Invalid property value

**Error:**
```
Invalid value for bundle.format: 'rar'
```

**Solution:**
Use valid values:
```properties
bundle.format = 7z  # or 'zip'
```

---

#### Issue: Path not found

**Error:**
```
Build path not found: C:/Invalid/Path
```

**Solution:**
1. Check path exists
2. Use forward slashes or escaped backslashes
3. Remove `build.path` to use default

```properties
# Correct
build.path = C:/Bearsampp-build

# Also correct
build.path = C:\\Bearsampp-build

# Use default
#build.path = C:/Bearsampp-build
```

---

**Last Updated:** 2025-08-20  
**Version:** 2025.8.20  
**Maintainer:** Bearsampp Team
