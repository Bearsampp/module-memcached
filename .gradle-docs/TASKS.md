# Gradle Tasks Reference

Complete reference for all Gradle tasks in the Bearsampp Module Memcached build system.

---

## Table of Contents

- [Task Groups](#task-groups)
- [Build Tasks](#build-tasks)
- [Verification Tasks](#verification-tasks)
- [Information Tasks](#information-tasks)
- [Task Dependencies](#task-dependencies)
- [Task Examples](#task-examples)

---

## Task Groups

Tasks are organized into logical groups:

| Group            | Purpose                                          |
|------------------|--------------------------------------------------|
| **build**        | Build and package tasks                          |
| **verification** | Verification and validation tasks                |
| **help**         | Help and information tasks                       |

---

## Build Tasks

### release

Build and package a release for a specific Memcached version.

**Group**: build

**Usage**:
```bash
# Interactive mode (prompts for version)
gradle release

# Non-interactive mode (specify version)
gradle release -PbundleVersion=1.6.29
```

**Parameters**:
- `-PbundleVersion=X.X.X` - Memcached version to build (optional, prompts if not provided)

**Description**:
- Validates the specified version exists in `bin/` or `bin/archived/`
- Copies Memcached files to preparation directory
- Creates archive in configured format (7z or zip)
- Generates hash files (MD5, SHA1, SHA256, SHA512)
- Outputs to `bearsampp-build/bins/memcached/{bundle.release}/`

**Output**:
```
bearsampp-build/bins/memcached/2025.8.20/
├── bearsampp-memcached-1.6.29-2025.8.20.7z
├── bearsampp-memcached-1.6.29-2025.8.20.7z.md5
├── bearsampp-memcached-1.6.29-2025.8.20.7z.sha1
├── bearsampp-memcached-1.6.29-2025.8.20.7z.sha256
└── bearsampp-memcached-1.6.29-2025.8.20.7z.sha512
```

**Example**:
```bash
# Build version 1.6.29
gradle release -PbundleVersion=1.6.29

# Interactive mode - prompts for version selection
gradle release
```

---

### releaseAll

Build releases for all available Memcached versions in the `bin/` directory.

**Group**: build

**Usage**:
```bash
gradle releaseAll
```

**Description**:
- Scans `bin/` and `bin/archived/` directories for all Memcached versions
- Builds each version sequentially
- Reports success/failure for each version
- Provides summary at the end

**Example**:
```bash
gradle releaseAll
```

**Output**:
```
======================================================================
Building releases for 3 memcached versions
======================================================================

[1/3] Building memcached 1.6.29...
[SUCCESS] memcached 1.6.29 completed

[2/3] Building memcached 1.6.39...
[SUCCESS] memcached 1.6.39 completed

[3/3] Building memcached 1.5.22...
[SUCCESS] memcached 1.5.22 completed

======================================================================
Build Summary
======================================================================
Total versions: 3
Successful:     3
Failed:         0
======================================================================
```

---

### clean

Clean build artifacts and temporary files.

**Group**: build

**Usage**:
```bash
gradle clean
```

**Description**:
- Removes Gradle build directory
- Cleans temporary build artifacts

**Example**:
```bash
gradle clean
```

---

## Verification Tasks

### verify

Verify the build environment and dependencies.

**Group**: verification

**Usage**:
```bash
gradle verify
```

**Description**:
Checks the following:
- Java version (8+)
- Required files (build.properties)
- Dev directory (optional warning)
- bin/ directory
- 7-Zip installation (if format is 7z)

**Example**:
```bash
gradle verify
```

**Output**:
```
Verifying build environment for module-memcached...

Environment Check Results:
------------------------------------------------------------
  [PASS]     Java 8+
  [PASS]     build.properties
  [FAIL]     dev directory
  [PASS]     bin directory
  [PASS]     7-Zip
------------------------------------------------------------

[SUCCESS] All checks passed! Build environment is ready.

You can now run:
  gradle release -PbundleVersion=1.6.29   - Build release for version
  gradle listVersions                     - List available versions
```

---

### validateProperties

Validate the build.properties configuration file.

**Group**: verification

**Usage**:
```bash
gradle validateProperties
```

**Description**:
Validates that all required properties are present:
- `bundle.name`
- `bundle.release`
- `bundle.type`
- `bundle.format`

**Example**:
```bash
gradle validateProperties
```

**Output**:
```
Validating build.properties...
[SUCCESS] All required properties are present:
    bundle.name = memcached
    bundle.release = 2025.8.20
    bundle.type = bins
    bundle.format = 7z
```

---

### checkModulesUntouched

Check integration with the modules-untouched repository.

**Group**: verification

**Usage**:
```bash
gradle checkModulesUntouched
```

**Description**:
- Fetches memcached.properties from modules-untouched repository
- Displays available versions
- Tests repository connectivity

**Example**:
```bash
gradle checkModulesUntouched
```

**Output**:
```
======================================================================
Modules-Untouched Integration Check
======================================================================

Repository URL:
  https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/memcached.properties

Fetching memcached.properties from modules-untouched...

======================================================================
Available Versions in modules-untouched
======================================================================
  1.6.29
  1.6.39
  1.5.22
======================================================================
Total versions: 3

======================================================================
[SUCCESS] modules-untouched integration is working
======================================================================
```

---

## Information Tasks

### info

Display build configuration information.

**Group**: help

**Usage**:
```bash
gradle info
```

**Description**:
Displays comprehensive build information including:
- Project details
- Bundle properties
- File paths
- Java and Gradle versions
- Available task groups
- Quick start commands

**Example**:
```bash
gradle info
```

**Output**:
```
================================================================
        Bearsampp Module Memcached - Build Info
================================================================

Project:        module-memcached
Version:        2025.8.20
Description:    Bearsampp Module - memcached

Bundle Properties:
  Name:         memcached
  Release:      2025.8.20
  Type:         bins
  Format:       7z

Paths:
  Project Dir:  E:/Bearsampp-development/module-memcached
  Root Dir:     E:/Bearsampp-development
  Build Base:   E:/Bearsampp-development/bearsampp-build
  Build Tmp:    E:/Bearsampp-development/bearsampp-build/tmp
  ...

Java:
  Version:      17
  Home:         C:/Program Files/Java/jdk-17

Gradle:
  Version:      8.5
  Home:         C:/Gradle/gradle-8.5

Quick Start:
  gradle tasks                              - List all available tasks
  gradle info                               - Show this information
  gradle release                            - Build release (interactive mode)
  gradle release -PbundleVersion=1.6.29     - Build release for version
  gradle releaseAll                         - Build all available versions
  gradle clean                              - Clean build artifacts
```

---

### listVersions

List all available Memcached versions in the `bin/` and `bin/archived/` directories.

**Group**: help

**Usage**:
```bash
gradle listVersions
```

**Description**:
- Scans `bin/` directory for Memcached versions
- Scans `bin/archived/` directory for archived versions
- Displays versions with location tags
- Shows total count

**Example**:
```bash
gradle listVersions
```

**Output**:
```
Available memcached versions:
------------------------------------------------------------
  1.6.29          [bin]
  1.6.39          [bin]
  1.5.22          [bin/archived]
------------------------------------------------------------
Total versions: 3

To build a specific version:
  gradle release -PbundleVersion=1.6.39
```

---

### listReleases

List all available Memcached releases from the modules-untouched repository.

**Group**: help

**Usage**:
```bash
gradle listReleases
```

**Description**:
- Fetches memcached.properties from modules-untouched
- Displays all available versions with download URLs
- Shows total count

**Example**:
```bash
gradle listReleases
```

**Output**:
```
Available Memcached Releases (modules-untouched):
--------------------------------------------------------------------------------
  1.6.29     -> https://memcached.org/files/memcached-1.6.29-win64.zip
  1.6.39     -> https://memcached.org/files/memcached-1.6.39-win64.zip
  1.5.22     -> https://memcached.org/files/memcached-1.5.22-win64.zip
--------------------------------------------------------------------------------
Total releases: 3
```

---

## Task Dependencies

### Task Execution Order

Tasks have implicit dependencies based on their functionality:

```
release
  └── (validates environment)
      └── (validates version exists)
          └── (copies files)
              └── (creates archive)
                  └── (generates hashes)

releaseAll
  └── (scans for versions)
      └── (calls release logic for each version)

verify
  └── (checks Java)
      └── (checks files)
          └── (checks directories)
              └── (checks 7-Zip)

validateProperties
  └── (loads build.properties)
      └── (validates required properties)

checkModulesUntouched
  └── (fetches remote properties)
      └── (displays versions)
```

---

## Task Examples

### Building a Single Version

```bash
# Interactive mode - prompts for version
gradle release

# Non-interactive mode - specify version
gradle release -PbundleVersion=1.6.29
```

### Building All Versions

```bash
# Build all versions in bin/ and bin/archived/
gradle releaseAll
```

### Verifying Environment

```bash
# Check if environment is ready for building
gradle verify

# Validate build.properties
gradle validateProperties

# Check modules-untouched integration
gradle checkModulesUntouched
```

### Getting Information

```bash
# Display build information
gradle info

# List local versions
gradle listVersions

# List remote versions
gradle listReleases

# List all available tasks
gradle tasks
```

### Cleaning Build Artifacts

```bash
# Clean build directory
gradle clean
```

### Debugging

```bash
# Run with info logging
gradle release -PbundleVersion=1.6.29 --info

# Run with debug logging
gradle release -PbundleVersion=1.6.29 --debug

# Run with stack trace on error
gradle release -PbundleVersion=1.6.29 --stacktrace
```

### Combining Tasks

```bash
# Clean and build
gradle clean release -PbundleVersion=1.6.29

# Verify and build
gradle verify release -PbundleVersion=1.6.29

# List versions and build
gradle listVersions release
```

---

## Task Options

### Global Gradle Options

| Option              | Description                                  |
|---------------------|----------------------------------------------|
| `--info`            | Set log level to INFO                        |
| `--debug`           | Set log level to DEBUG                       |
| `--stacktrace`      | Print stack trace on error                   |
| `--scan`            | Create build scan                            |
| `--offline`         | Execute build without network access         |
| `--refresh-dependencies` | Refresh cached dependencies           |

### Project Properties

| Property            | Description                                  | Example                          |
|---------------------|----------------------------------------------|----------------------------------|
| `-PbundleVersion`   | Specify Memcached version to build           | `-PbundleVersion=1.6.29`         |

---

## Task Output

### Success Output

```
======================================================================
[SUCCESS] Release build completed successfully for version 1.6.29
Output directory: E:/Bearsampp-development/bearsampp-build/tmp/bundles_build/bins/memcached/memcached1.6.29
Archive: E:/Bearsampp-development/bearsampp-build/bins/memcached/2025.8.20/bearsampp-memcached-1.6.29-2025.8.20.7z
======================================================================
```

### Error Output

```
FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':release'.
> Bundle version not found in bin/ or bin/archived/

Available versions:
  - 1.6.29
  - 1.6.39
  - 1.5.22
```

---

## Best Practices

### Task Usage

1. **Always verify first**: Run `gradle verify` before building
2. **List versions**: Use `gradle listVersions` to see available versions
3. **Use non-interactive mode in scripts**: Specify `-PbundleVersion` for automation
4. **Clean when needed**: Run `gradle clean` if you encounter issues
5. **Check logs**: Use `--info` or `--debug` for troubleshooting

### Performance Tips

1. **Enable Gradle daemon**: Set `org.gradle.daemon=true` in gradle.properties
2. **Use parallel execution**: Set `org.gradle.parallel=true`
3. **Enable caching**: Set `org.gradle.caching=true`
4. **Increase heap size**: Set `org.gradle.jvmargs=-Xmx2g`

---

## Support

For task-related issues:

- **Documentation**: [README.md](README.md)
- **Configuration**: [CONFIGURATION.md](CONFIGURATION.md)
- **GitHub Issues**: https://github.com/bearsampp/module-memcached/issues
- **Bearsampp Issues**: https://github.com/bearsampp/bearsampp/issues

---

**Last Updated**: 2025-08-20  
**Version**: 2025.8.20  
**Build System**: Pure Gradle
