# Migration Guide: Ant to Gradle

Complete guide for migrating from the legacy Ant build system to the modern Gradle build system.

---

## Table of Contents

- [Overview](#overview)
- [What Changed](#what-changed)
- [Command Mapping](#command-mapping)
- [File Changes](#file-changes)
- [Configuration Changes](#configuration-changes)
- [Task Equivalents](#task-equivalents)
- [Benefits of Migration](#benefits-of-migration)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

---

## Overview

The Bearsampp Module Memcached project has been fully migrated from Apache Ant to Gradle. This migration provides:

- **Modern Build System**: Native Gradle features and conventions
- **Better Performance**: Incremental builds, caching, and parallel execution
- **Simplified Maintenance**: Pure Groovy/Gradle DSL instead of XML
- **Enhanced Tooling**: Better IDE integration and dependency management
- **Cross-Platform Support**: Consistent behavior across Windows, Linux, and macOS

### Migration Status

| Component         | Status      | Notes                                    |
|-------------------|-------------|------------------------------------------|
| Build System      | ✅ Complete | Fully migrated to Gradle                 |
| Build Scripts     | ✅ Complete | build.xml removed, build.gradle created  |
| Configuration     | ✅ Complete | build.properties retained and enhanced   |
| Tasks             | ✅ Complete | All Ant targets converted to Gradle tasks|
| Documentation     | ✅ Complete | Updated for Gradle                       |

---

## What Changed

### Removed Components

| Component         | Status      | Replacement                              |
|-------------------|-------------|------------------------------------------|
| `build.xml`       | ❌ Removed   | `build.gradle`                           |
| Ant tasks         | ❌ Removed   | Gradle tasks                             |
| Ant properties    | ❌ Removed   | Gradle properties and ext variables      |

### New Components

| Component         | Status      | Purpose                                  |
|-------------------|-------------|------------------------------------------|
| `build.gradle`    | ✅ Added     | Main Gradle build script                 |
| `settings.gradle` | ✅ Added     | Gradle project settings                  |
| `gradle.properties` | ✅ Added   | Gradle-specific configuration            |
| `.gradle-docs/`   | ✅ Added     | Comprehensive Gradle documentation       |

### Retained Components

| Component         | Status      | Notes                                    |
|-------------------|-------------|------------------------------------------|
| `build.properties`| ✅ Retained  | Enhanced with new options                |
| `releases.properties` | ✅ Retained | Release history tracking              |
| `bin/` directory  | ✅ Retained  | Memcached version bundles                |

---

## Command Mapping

### Build Commands

| Ant Command                          | Gradle Command                              | Notes |
|--------------------------------------|---------------------------------------------|-------|
| `ant release`                        | `gradle release`                            | Interactive mode |
| `ant release -Dinput.bundle=1.6.29`  | `gradle release -PbundleVersion=1.6.29`     | Non-interactive mode |
| `ant clean`                          | `gradle clean`                              | Clean build artifacts |
| N/A                                  | `gradle releaseAll`                         | New: Build all versions |

### Information Commands

| Ant Command                          | Gradle Command                              | Notes |
|--------------------------------------|---------------------------------------------|-------|
| N/A                                  | `gradle info`                               | New: Display build info |
| N/A                                  | `gradle tasks`                              | New: List all tasks |
| N/A                                  | `gradle listVersions`                       | New: List local versions |
| N/A                                  | `gradle listReleases`                       | New: List remote versions |

### Verification Commands

| Ant Command                          | Gradle Command                              | Notes |
|--------------------------------------|---------------------------------------------|-------|
| N/A                                  | `gradle verify`                             | New: Verify environment |
| N/A                                  | `gradle validateProperties`                 | New: Validate config |
| N/A                                  | `gradle checkModulesUntouched`              | New: Check integration |

---

## File Changes

### Build Scripts

#### Before (Ant)

**File**: `build.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="module-memcached" default="release">
    <property file="build.properties"/>
    
    <target name="release">
        <!-- Ant build logic -->
    </target>
    
    <target name="clean">
        <!-- Ant clean logic -->
    </target>
</project>
```

#### After (Gradle)

**File**: `build.gradle`

```groovy
plugins {
    id 'base'
}

// Load build properties
def buildProps = new Properties()
file('build.properties').withInputStream { buildProps.load(it) }

// Project information
group = 'com.bearsampp.modules'
version = buildProps.getProperty('bundle.release', '1.0.0')

// Tasks
tasks.register('release') {
    group = 'build'
    description = 'Build release package'
    // Gradle build logic
}

tasks.named('clean') {
    group = 'build'
    description = 'Clean build artifacts'
    // Gradle clean logic
}
```

### Configuration Files

#### build.properties

**Before (Ant)**:
```properties
bundle.name = memcached
bundle.release = 2025.8.20
bundle.type = bins
bundle.format = 7z
```

**After (Gradle)**:
```properties
bundle.name = memcached
bundle.release = 2025.8.20
bundle.type = bins
bundle.format = 7z

# Optional: Custom build path
# build.path = C:/Bearsampp-build
```

**Changes**:
- ✅ All existing properties retained
- ✅ New optional `build.path` property added
- ✅ Better documentation and comments

---

## Configuration Changes

### Property Access

#### Before (Ant)

```xml
<property name="bundle.name" value="memcached"/>
<echo message="Building ${bundle.name}"/>
```

#### After (Gradle)

```groovy
def buildProps = new Properties()
file('build.properties').withInputStream { buildProps.load(it) }

ext {
    bundleName = buildProps.getProperty('bundle.name', 'memcached')
}

println "Building ${bundleName}"
```

### Path Configuration

#### Before (Ant)

```xml
<property name="build.path" value="C:/Bearsampp-build"/>
```

#### After (Gradle)

```groovy
// Priority: 1) build.properties, 2) Environment variable, 3) Default
def buildPathFromProps = buildProps.getProperty('build.path', '').trim()
def buildPathFromEnv = System.getenv('BEARSAMPP_BUILD_PATH') ?: ''
def defaultBuildPath = "${rootDir}/bearsampp-build"

buildBasePath = buildPathFromProps ?: (buildPathFromEnv ?: defaultBuildPath)
```

**Improvements**:
- ✅ Multiple configuration sources
- ✅ Environment variable support
- ✅ Sensible defaults
- ✅ Clear priority order

---

## Task Equivalents

### Release Task

#### Before (Ant)

```xml
<target name="release">
    <input message="Enter version:" addproperty="input.bundle"/>
    <copy todir="${build.path}">
        <fileset dir="bin/${bundle.name}${input.bundle}"/>
    </copy>
    <zip destfile="${build.path}/${bundle.name}-${input.bundle}.zip">
        <fileset dir="${build.path}"/>
    </zip>
</target>
```

#### After (Gradle)

```groovy
tasks.register('release') {
    group = 'build'
    description = 'Build release package'
    
    doLast {
        def versionToBuild = project.findProperty('bundleVersion')
        
        if (!versionToBuild) {
            // Interactive mode with version selection
            def availableVersions = getAvailableVersions()
            // ... prompt logic ...
        }
        
        // Copy files
        copy {
            from bundleSrcDest
            into memcachedPrepPath
        }
        
        // Create archive with hash files
        // ... archive creation logic ...
        generateHashFiles(archiveFile)
    }
}
```

**Improvements**:
- ✅ Better interactive mode with version listing
- ✅ Non-interactive mode support
- ✅ Automatic hash file generation
- ✅ Better error handling
- ✅ Progress reporting

### Clean Task

#### Before (Ant)

```xml
<target name="clean">
    <delete dir="${build.path}"/>
</target>
```

#### After (Gradle)

```groovy
tasks.named('clean') {
    group = 'build'
    description = 'Clean build artifacts'
    
    doLast {
        def buildDir = file("${projectDir}/build")
        if (buildDir.exists()) {
            delete buildDir
        }
        println "[SUCCESS] Build artifacts cleaned"
    }
}
```

**Improvements**:
- ✅ Uses Gradle's built-in clean task
- ✅ Better feedback
- ✅ Safer deletion logic

---

## Benefits of Migration

### Performance

| Feature              | Ant          | Gradle       | Improvement |
|----------------------|--------------|--------------|-------------|
| Incremental Builds   | ❌ No        | ✅ Yes       | Faster rebuilds |
| Build Caching        | ❌ No        | ✅ Yes       | Reuse previous builds |
| Parallel Execution   | ❌ No        | ✅ Yes       | Faster multi-task builds |
| Daemon Mode          | ❌ No        | ✅ Yes       | Faster startup |

### Developer Experience

| Feature              | Ant          | Gradle       | Improvement |
|----------------------|--------------|--------------|-------------|
| IDE Integration      | ⚠️ Limited   | ✅ Excellent | Better tooling |
| Task Discovery       | ⚠️ Manual    | ✅ Built-in  | `gradle tasks` |
| Dependency Management| ❌ Manual    | ✅ Automatic | Easier updates |
| Error Messages       | ⚠️ Basic     | ✅ Detailed  | Better debugging |

### Maintainability

| Feature              | Ant          | Gradle       | Improvement |
|----------------------|--------------|--------------|-------------|
| Build Script Language| XML          | Groovy DSL   | More readable |
| Code Reuse           | ⚠️ Limited   | ✅ Excellent | Functions, plugins |
| Testing              | ❌ Difficult | ✅ Easy      | Built-in support |
| Documentation        | ⚠️ External  | ✅ Integrated| Task descriptions |

### Features

| Feature              | Ant          | Gradle       | Notes |
|----------------------|--------------|--------------|-------|
| Interactive Mode     | ⚠️ Basic     | ✅ Enhanced  | Version selection |
| Batch Building       | ❌ No        | ✅ Yes       | `releaseAll` task |
| Hash Generation      | ❌ Manual    | ✅ Automatic | MD5, SHA1, SHA256, SHA512 |
| Environment Checks   | ❌ No        | ✅ Yes       | `verify` task |
| Version Listing      | ❌ No        | ✅ Yes       | `listVersions` task |

---

## Troubleshooting

### Common Migration Issues

#### Issue: "build.xml not found"

**Symptom**: Old scripts or documentation reference `build.xml`

**Solution**:
- `build.xml` has been removed
- Use `build.gradle` instead
- Update any scripts or documentation

#### Issue: "Ant command not working"

**Symptom**: `ant release` fails

**Solution**:
- Ant is no longer used
- Use Gradle commands instead
- See [Command Mapping](#command-mapping)

#### Issue: "Properties not found"

**Symptom**: Build fails with missing properties

**Solution**:
1. Verify `build.properties` exists
2. Run `gradle validateProperties`
3. Check property names match exactly

#### Issue: "Different output location"

**Symptom**: Archives not in expected location

**Solution**:
- Gradle uses `bearsampp-build/` structure
- Configure with `build.path` in build.properties
- Or set `BEARSAMPP_BUILD_PATH` environment variable

---

## Next Steps

### For Developers

1. **Install Gradle**: Download from https://gradle.org/
2. **Verify Environment**: Run `gradle verify`
3. **Learn Commands**: Review [Command Mapping](#command-mapping)
4. **Read Documentation**: See [.gradle-docs/README.md](README.md)
5. **Try Building**: Run `gradle release`

### For CI/CD

1. **Update Scripts**: Replace Ant commands with Gradle
2. **Install Gradle**: Add to CI environment
3. **Use Non-Interactive Mode**: Always specify `-PbundleVersion`
4. **Cache Gradle**: Cache `.gradle` directory for faster builds
5. **Test Builds**: Verify all versions build correctly

### For Contributors

1. **Read Build Script**: Review `build.gradle`
2. **Understand Tasks**: See [TASKS.md](TASKS.md)
3. **Learn Configuration**: See [CONFIGURATION.md](CONFIGURATION.md)
4. **Follow Conventions**: Use Gradle best practices
5. **Update Documentation**: Keep docs in sync with changes

---

## Migration Checklist

Use this checklist to ensure complete migration:

### Build System

- [x] Remove `build.xml`
- [x] Create `build.gradle`
- [x] Create `settings.gradle`
- [x] Create `gradle.properties`
- [x] Test all build tasks

### Configuration

- [x] Verify `build.properties` works
- [x] Add optional properties
- [x] Test environment variables
- [x] Validate configuration

### Documentation

- [x] Update README.md
- [x] Create .gradle-docs/
- [x] Write task documentation
- [x] Write configuration guide
- [x] Write migration guide

### Testing

- [x] Test release task
- [x] Test releaseAll task
- [x] Test clean task
- [x] Test verification tasks
- [x] Test information tasks

### Cleanup

- [x] Remove Ant references
- [x] Update scripts
- [x] Update CI/CD
- [x] Archive old documentation

---

## Support

For migration assistance:

- **Documentation**: [README.md](README.md)
- **Tasks Reference**: [TASKS.md](TASKS.md)
- **Configuration**: [CONFIGURATION.md](CONFIGURATION.md)
- **GitHub Issues**: https://github.com/bearsampp/module-memcached/issues
- **Bearsampp Issues**: https://github.com/bearsampp/bearsampp/issues

---

## Additional Resources

### Gradle

- [Gradle Documentation](https://docs.gradle.org/)
- [Gradle User Manual](https://docs.gradle.org/current/userguide/userguide.html)
- [Gradle Build Language Reference](https://docs.gradle.org/current/dsl/)
- [Migrating from Ant](https://docs.gradle.org/current/userguide/migrating_from_ant.html)

### Bearsampp

- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [Bearsampp Website](https://bearsampp.com)
- [Bearsampp Documentation](https://bearsampp.com/docs)

---

**Last Updated**: 2025-08-20  
**Version**: 2025.8.20  
**Migration Status**: Complete
