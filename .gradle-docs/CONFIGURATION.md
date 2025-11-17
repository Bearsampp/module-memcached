# Configuration Guide

Complete guide to configuring the Bearsampp Module Memcached build system.

---

## Table of Contents

- [Configuration Files](#configuration-files)
- [Build Properties](#build-properties)
- [Gradle Properties](#gradle-properties)
- [Release Properties](#release-properties)
- [Environment Variables](#environment-variables)
- [Build Paths](#build-paths)
- [Archive Configuration](#archive-configuration)
- [Configuration Examples](#configuration-examples)
- [Best Practices](#best-practices)

---

## Configuration Files

The build system uses several configuration files:

| File                  | Purpose                                  | Required |
|-----------------------|------------------------------------------|----------|
| `build.properties`    | Main build configuration                 | Yes      |
| `gradle.properties`   | Gradle-specific settings                 | No       |
| `releases.properties` | Release history tracking                 | No       |
| `settings.gradle`     | Gradle project settings                  | Yes      |

---

## Build Properties

### File: build.properties

Main configuration file for the build system.

**Location**: `build.properties` (project root)

**Format**: Java properties file

**Required Properties**:

| Property          | Description                          | Example Value  | Required |
|-------------------|--------------------------------------|----------------|----------|
| `bundle.name`     | Name of the bundle                   | `memcached`    | Yes      |
| `bundle.release`  | Release version                      | `2025.8.20`    | Yes      |
| `bundle.type`     | Type of bundle                       | `bins`         | Yes      |
| `bundle.format`   | Archive format (7z or zip)           | `7z`           | Yes      |

**Optional Properties**:

| Property          | Description                          | Example Value  | Default |
|-------------------|--------------------------------------|----------------|---------|
| `build.path`      | Custom build output path             | `C:/Bearsampp-build` | `{parent}/bearsampp-build` |

**Example**:

```properties
# Bundle configuration
bundle.name     = memcached
bundle.release  = 2025.8.20
bundle.type     = bins
bundle.format   = 7z

# Optional: Custom build path
# build.path = C:/Bearsampp-build
```

### Property Details

#### bundle.name

The name of the module bundle.

- **Type**: String
- **Required**: Yes
- **Default**: None
- **Example**: `memcached`
- **Usage**: Used in archive names and directory paths

#### bundle.release

The release version for the build.

- **Type**: String (date format recommended: YYYY.M.D)
- **Required**: Yes
- **Default**: None
- **Example**: `2025.8.20`
- **Usage**: Used in archive names and output directories

#### bundle.type

The type of bundle being built.

- **Type**: String
- **Required**: Yes
- **Default**: None
- **Allowed Values**: `bins`, `tools`
- **Example**: `bins`
- **Usage**: Determines output directory structure

#### bundle.format

The archive format for the release package.

- **Type**: String
- **Required**: Yes
- **Default**: None
- **Allowed Values**: `7z`, `zip`
- **Example**: `7z`
- **Usage**: Determines compression format and tool used

**Format Comparison**:

| Format | Compression | Speed  | Tool Required | Recommended |
|--------|-------------|--------|---------------|-------------|
| `7z`   | Excellent   | Slower | 7-Zip         | Yes         |
| `zip`  | Good        | Faster | Built-in      | No          |

#### build.path

Custom build output path (optional).

- **Type**: String (absolute path)
- **Required**: No
- **Default**: `{parent}/bearsampp-build`
- **Example**: `C:/Bearsampp-build`
- **Usage**: Override default build output location

**Priority Order**:
1. `build.properties` value
2. `BEARSAMPP_BUILD_PATH` environment variable
3. Default: `{parent}/bearsampp-build`

---

## Gradle Properties

### File: gradle.properties

Gradle-specific configuration settings.

**Location**: `gradle.properties` (project root)

**Format**: Java properties file

**Recommended Settings**:

```properties
# Gradle daemon configuration
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true

# JVM settings
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m -XX:+HeapDumpOnOutOfMemoryError

# Console output
org.gradle.console=auto
org.gradle.warning.mode=all
```

### Property Details

#### Daemon Settings

| Property              | Description                          | Recommended |
|-----------------------|--------------------------------------|-------------|
| `org.gradle.daemon`   | Enable Gradle daemon                 | `true`      |
| `org.gradle.parallel` | Enable parallel execution            | `true`      |
| `org.gradle.caching`  | Enable build caching                 | `true`      |

#### JVM Settings

| Property                  | Description                      | Recommended |
|---------------------------|----------------------------------|-------------|
| `org.gradle.jvmargs`      | JVM arguments for Gradle         | `-Xmx2g -XX:MaxMetaspaceSize=512m` |

**Common JVM Arguments**:
- `-Xmx2g` - Maximum heap size (2GB)
- `-Xms512m` - Initial heap size (512MB)
- `-XX:MaxMetaspaceSize=512m` - Maximum metaspace size
- `-XX:+HeapDumpOnOutOfMemoryError` - Create heap dump on OOM

#### Console Settings

| Property                  | Description                      | Options |
|---------------------------|----------------------------------|---------|
| `org.gradle.console`      | Console output mode              | `auto`, `plain`, `rich`, `verbose` |
| `org.gradle.warning.mode` | Warning display mode             | `all`, `summary`, `none` |

---

## Release Properties

### File: releases.properties

Tracks release history and metadata.

**Location**: `releases.properties` (project root)

**Format**: Java properties file

**Purpose**: Historical tracking of releases

**Example**:

```properties
# Release history
2025.8.20 = Released on 2025-08-20
2025.7.15 = Released on 2025-07-15
2025.6.10 = Released on 2025-06-10
```

---

## Environment Variables

The build system supports several environment variables:

### BEARSAMPP_BUILD_PATH

Override the default build output path.

- **Type**: String (absolute path)
- **Required**: No
- **Default**: `{parent}/bearsampp-build`
- **Example**: `C:/Bearsampp-build`
- **Priority**: 2 (after build.properties, before default)

**Usage**:

```bash
# Windows (PowerShell)
$env:BEARSAMPP_BUILD_PATH = "C:/Bearsampp-build"
gradle release -PbundleVersion=1.6.29

# Windows (CMD)
set BEARSAMPP_BUILD_PATH=C:/Bearsampp-build
gradle release -PbundleVersion=1.6.29

# Linux/macOS
export BEARSAMPP_BUILD_PATH=/opt/bearsampp-build
gradle release -PbundleVersion=1.6.29
```

### 7Z_HOME

Specify the 7-Zip installation directory.

- **Type**: String (absolute path)
- **Required**: No (if 7-Zip is in PATH or standard location)
- **Example**: `C:/Program Files/7-Zip`

**Usage**:

```bash
# Windows (PowerShell)
$env:7Z_HOME = "C:/Program Files/7-Zip"
gradle release -PbundleVersion=1.6.29

# Windows (CMD)
set 7Z_HOME=C:/Program Files/7-Zip
gradle release -PbundleVersion=1.6.29
```

### JAVA_HOME

Specify the Java installation directory.

- **Type**: String (absolute path)
- **Required**: Yes (usually set by Java installer)
- **Example**: `C:/Program Files/Java/jdk-17`

**Usage**:

```bash
# Windows (PowerShell)
$env:JAVA_HOME = "C:/Program Files/Java/jdk-17"
gradle release -PbundleVersion=1.6.29
```

---

## Build Paths

The build system uses a structured directory layout:

### Default Path Structure

```
bearsampp-build/                    # Build base directory
├── tmp/                            # Temporary build files
│   ├── bundles_prep/               # Preparation directory
│   │   └── bins/memcached/         # Prepared bundles
│   │       └── memcached1.6.29/    # Version-specific prep
│   └── bundles_build/              # Build staging directory
│       └── bins/memcached/         # Staged bundles
│           └── memcached1.6.29/    # Version-specific build
└── bins/memcached/                 # Final output directory
    └── 2025.8.20/                  # Release version
        ├── bearsampp-memcached-1.6.29-2025.8.20.7z
        ├── bearsampp-memcached-1.6.29-2025.8.20.7z.md5
        ├── bearsampp-memcached-1.6.29-2025.8.20.7z.sha1
        ├── bearsampp-memcached-1.6.29-2025.8.20.7z.sha256
        └── bearsampp-memcached-1.6.29-2025.8.20.7z.sha512
```

### Path Variables

| Variable              | Description                          | Example |
|-----------------------|--------------------------------------|---------|
| `buildBasePath`       | Base build directory                 | `E:/Bearsampp-development/bearsampp-build` |
| `buildTmpPath`        | Temporary files directory            | `{buildBasePath}/tmp` |
| `bundleTmpPrepPath`   | Bundle preparation directory         | `{buildTmpPath}/bundles_prep/bins/memcached` |
| `bundleTmpBuildPath`  | Bundle build staging directory       | `{buildTmpPath}/bundles_build/bins/memcached` |
| `buildBinsPath`       | Final output directory               | `{buildBasePath}/bins/memcached/{bundle.release}` |

### Customizing Build Paths

**Method 1: build.properties**

```properties
build.path = C:/Custom/Build/Path
```

**Method 2: Environment Variable**

```bash
export BEARSAMPP_BUILD_PATH=/opt/custom/build
```

**Method 3: Default**

Uses `{parent}/bearsampp-build` automatically.

---

## Archive Configuration

### Archive Naming

Archives follow this naming convention:

```
bearsampp-{bundle.name}-{version}-{bundle.release}.{bundle.format}
```

**Examples**:
- `bearsampp-memcached-1.6.29-2025.8.20.7z`
- `bearsampp-memcached-1.6.39-2025.8.20.zip`

### Archive Structure

Archives contain a top-level version folder:

```
bearsampp-memcached-1.6.29-2025.8.20.7z
└── memcached1.6.29/               ← Version folder at root
    ├── memcached.exe
    ├── memcached.conf
    └── ...
```

### Hash Files

Each archive is accompanied by hash files:

| File Extension | Algorithm | Purpose |
|----------------|-----------|---------|
| `.md5`         | MD5       | Quick verification |
| `.sha1`        | SHA-1     | Legacy compatibility |
| `.sha256`      | SHA-256   | Recommended verification |
| `.sha512`      | SHA-512   | Maximum security |

**Hash File Format**:

```
{hash} {filename}
```

**Example** (`.sha256`):

```
a1b2c3d4e5f6... bearsampp-memcached-1.6.29-2025.8.20.7z
```

---

## Configuration Examples

### Example 1: Standard Configuration

```properties
# build.properties
bundle.name     = memcached
bundle.release  = 2025.8.20
bundle.type     = bins
bundle.format   = 7z
```

```properties
# gradle.properties
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m
```

### Example 2: Custom Build Path

```properties
# build.properties
bundle.name     = memcached
bundle.release  = 2025.8.20
bundle.type     = bins
bundle.format   = 7z
build.path      = C:/Bearsampp-build
```

### Example 3: ZIP Format

```properties
# build.properties
bundle.name     = memcached
bundle.release  = 2025.8.20
bundle.type     = bins
bundle.format   = zip
```

### Example 4: Development Configuration

```properties
# build.properties
bundle.name     = memcached
bundle.release  = 2025.8.20-dev
bundle.type     = bins
bundle.format   = zip
build.path      = C:/Dev/Bearsampp-build
```

```properties
# gradle.properties
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.jvmargs=-Xmx4g -XX:MaxMetaspaceSize=1g
org.gradle.console=verbose
org.gradle.warning.mode=all
```

---

## Best Practices

### Configuration Management

1. **Version Control**: Commit `build.properties` and `gradle.properties`
2. **Documentation**: Document any custom configurations
3. **Consistency**: Use consistent naming and versioning
4. **Validation**: Run `gradle validateProperties` after changes

### Build Properties

1. **Release Versioning**: Use date format (YYYY.M.D) for `bundle.release`
2. **Archive Format**: Prefer `7z` for better compression
3. **Build Path**: Use absolute paths for `build.path`
4. **Comments**: Add comments to explain custom settings

### Gradle Properties

1. **Enable Daemon**: Always set `org.gradle.daemon=true`
2. **Parallel Execution**: Enable for faster builds
3. **Caching**: Enable for incremental builds
4. **Heap Size**: Adjust based on available memory

### Environment Variables

1. **Persistence**: Set in system environment for permanent use
2. **Documentation**: Document required environment variables
3. **Validation**: Verify environment variables are set correctly
4. **Priority**: Understand precedence order

### Path Configuration

1. **Absolute Paths**: Always use absolute paths
2. **Forward Slashes**: Use `/` instead of `\` for cross-platform compatibility
3. **No Trailing Slash**: Don't end paths with `/`
4. **Validation**: Ensure paths exist and are writable

---

## Troubleshooting Configuration

### Issue: Invalid Properties

**Symptom**: `build.properties validation failed`

**Solution**:
1. Run `gradle validateProperties`
2. Check for missing required properties
3. Verify property values are valid
4. Check for typos in property names

### Issue: Build Path Not Found

**Symptom**: `Build path not found` or permission errors

**Solution**:
1. Verify path exists and is writable
2. Use absolute paths
3. Check environment variable is set correctly
4. Ensure no trailing slashes

### Issue: 7-Zip Not Found

**Symptom**: `7-Zip executable not found!`

**Solution**:
1. Install 7-Zip from https://www.7-zip.org/
2. Set `7Z_HOME` environment variable
3. Or change `bundle.format=zip` in build.properties

### Issue: Gradle Daemon Issues

**Symptom**: Slow builds or memory errors

**Solution**:
1. Stop daemon: `gradle --stop`
2. Adjust heap size in gradle.properties
3. Disable daemon temporarily: `gradle --no-daemon`

---

## Support

For configuration issues:

- **Documentation**: [README.md](README.md)
- **Tasks Reference**: [TASKS.md](TASKS.md)
- **GitHub Issues**: https://github.com/bearsampp/module-memcached/issues
- **Bearsampp Issues**: https://github.com/bearsampp/bearsampp/issues

---

**Last Updated**: 2025-08-20  
**Version**: 2025.8.20  
**Build System**: Pure Gradle
