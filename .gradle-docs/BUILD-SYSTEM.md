# Build System Specification

## Overview

The Bearsampp Module Memcached uses a **pure Gradle build system** with the following specifications:

## Build System Details

| Aspect                    | Specification                                    |
|--------------------------|--------------------------------------------------|
| **Build Tool**           | Gradle (system-installed)                        |
| **Gradle Version**       | 7.0+ (tested with 9.2.0)                         |
| **DSL Language**         | Groovy                                           |
| **Wrapper**              | Not used (requires system Gradle)                |
| **Build Files**          | `build.gradle`, `settings.gradle` (Groovy)       |
| **Configuration Files**  | `build.properties`, `gradle.properties`          |

## File Structure

### Build Files

```
module-memcached/
├── build.gradle              # Main build script (Groovy DSL)
├── settings.gradle           # Gradle settings (Groovy DSL)
├── gradle.properties         # Gradle configuration
└── build.properties          # Bundle configuration
```

### No Wrapper Files

The following files are **NOT** present (no wrapper used):

- ❌ `gradlew` (Unix wrapper script)
- ❌ `gradlew.bat` (Windows wrapper script)
- ❌ `gradle/wrapper/gradle-wrapper.jar`
- ❌ `gradle/wrapper/gradle-wrapper.properties`

### No Kotlin Files

The following files are **NOT** present (Groovy DSL only):

- ❌ `build.gradle.kts` (Kotlin DSL)
- ❌ `settings.gradle.kts` (Kotlin DSL)
- ❌ Any `.kts` files

## Groovy DSL Syntax

### build.gradle (Groovy)

```groovy
plugins {
    id 'base'
    id 'distribution'
}

// Load build properties
def buildProps = new Properties()
file('build.properties').withInputStream { buildProps.load(it) }

// Project information
group = 'com.bearsampp.modules'
version = buildProps.getProperty('bundle.release', '1.0.0')

// Define project paths
ext {
    projectBasedir = projectDir.absolutePath
    bundleName = buildProps.getProperty('bundle.name', 'memcached')
}

// Task definitions
tasks.register('info') {
    group = 'help'
    description = 'Display build information'
    
    doLast {
        println "Project: ${project.name}"
    }
}
```

### settings.gradle (Groovy)

```groovy
rootProject.name = 'module-memcached'

enableFeaturePreview('STABLE_CONFIGURATION_CACHE')

buildCache {
    local {
        enabled = true
        directory = file("${rootDir}/.gradle/build-cache")
    }
}
```

## Why Groovy DSL?

### Advantages

| Advantage                 | Description                                      |
|--------------------------|--------------------------------------------------|
| **Mature**               | Stable, well-established DSL                     |
| **Flexible**             | Dynamic typing, easier scripting                 |
| **Compatible**           | Works with all Gradle versions                   |
| **Readable**             | Cleaner syntax for build scripts                 |
| **Documentation**        | More examples and resources available            |

### Comparison: Groovy vs Kotlin DSL

| Aspect                | Groovy DSL                | Kotlin DSL                |
|----------------------|---------------------------|---------------------------|
| File Extension       | `.gradle`                 | `.gradle.kts`             |
| Syntax               | Dynamic, flexible         | Static, type-safe         |
| IDE Support          | Good                      | Better (IntelliJ)         |
| Build Performance    | Fast                      | Slower (compilation)      |
| Learning Curve       | Easier                    | Steeper                   |
| Maturity             | Very mature               | Newer                     |

## System Requirements

### Required

| Requirement          | Version      | Purpose                                    |
|---------------------|--------------|---------------------------------------------|
| Java                | 8+           | Run Gradle                                  |
| Gradle              | 7.0+         | Build automation (system-installed)         |
| 7-Zip               | Latest       | Create .7z archives                         |

### Installation

#### Install Gradle (System-wide)

**Windows:**

```powershell
# Using Chocolatey
choco install gradle

# Using Scoop
scoop install gradle

# Manual installation
# 1. Download from https://gradle.org/releases/
# 2. Extract to C:\Gradle
# 3. Add C:\Gradle\bin to PATH
```

**Verify Installation:**

```bash
gradle --version
```

Expected output:

```
------------------------------------------------------------
Gradle 9.2.0
------------------------------------------------------------

Build time:    2025-10-29 13:53:23 UTC
Revision:      d9d6bbce03b3d88c67ef5a0ff31f7ae5e332d6bf

Kotlin:        2.2.20
Groovy:        4.0.28
Ant:           Apache Ant(TM) version 1.10.15 compiled on August 25 2024
Launcher JVM:  25.0.1 (Microsoft 25.0.1+8-LTS)
Daemon JVM:    C:\Program Files\Microsoft\jdk-25.0.1.8-hotspot
OS:            Windows 11 10.0 amd64
```

## Build Commands

All commands use the system-installed `gradle` command:

```bash
# Display build information
gradle info

# List available tasks
gradle tasks

# Build release
gradle release -PbundleVersion=1.6.39

# Clean build artifacts
gradle clean

# Verify environment
gradle verify
```

## Configuration

### gradle.properties

Gradle daemon and performance settings:

```properties
# Enable Gradle daemon for faster builds
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

# File system watching (Gradle 7.0+)
org.gradle.vfs.watch=true
```

### build.properties

Bundle-specific configuration:

```properties
bundle.name = memcached
bundle.release = 2025.8.20
bundle.type = bins
bundle.format = 7z
```

## Build Process

### 1. Configuration Phase

```
gradle command
    ↓
Load settings.gradle (Groovy)
    ↓
Load build.gradle (Groovy)
    ↓
Load build.properties
    ↓
Configure project
```

### 2. Execution Phase

```
Execute requested task
    ↓
Run task actions
    ↓
Generate output
```

## Verification

### Check Build System

```bash
# Verify Gradle installation
gradle --version

# Verify no wrapper files
ls gradlew* 2>$null
ls gradle/wrapper 2>$null

# Verify Groovy DSL files
ls *.gradle

# Verify no Kotlin DSL files
ls *.gradle.kts 2>$null
```

### Expected Results

```bash
# gradle --version
✓ Shows Gradle version with Groovy

# ls gradlew*
✗ No wrapper files found

# ls *.gradle
✓ build.gradle
✓ settings.gradle

# ls *.gradle.kts
✗ No Kotlin DSL files found
```

## Migration Notes

### From Ant

- ✓ Removed `build.xml`
- ✓ Converted to pure Gradle (Groovy DSL)
- ✓ No external Ant dependencies

### From Gradle Wrapper

- ✓ No `gradlew` scripts
- ✓ No `gradle/wrapper/` directory
- ✓ Uses system-installed Gradle

### From Kotlin DSL

- ✓ No `.gradle.kts` files
- ✓ Uses Groovy DSL (`.gradle` files)
- ✓ Dynamic, flexible syntax

## Best Practices

### Groovy DSL

1. **Use single quotes** for strings (unless interpolation needed)
   ```groovy
   id 'base'  // Good
   id "base"  // Works but unnecessary
   ```

2. **Use method call syntax** without parentheses
   ```groovy
   println 'Hello'  // Good
   println('Hello') // Works but verbose
   ```

3. **Use closures** for configuration
   ```groovy
   tasks.register('myTask') {
       doLast {
           println 'Task executed'
       }
   }
   ```

4. **Use ext for extra properties**
   ```groovy
   ext {
       myProperty = 'value'
   }
   ```

### System Gradle

1. **Keep Gradle updated** on development machines
2. **Document required Gradle version** in README
3. **Use version-compatible features** (7.0+)
4. **Test with multiple Gradle versions** if possible

## Troubleshooting

### Issue: Gradle not found

**Error:**
```
'gradle' is not recognized as an internal or external command
```

**Solution:**
Install Gradle system-wide and add to PATH.

### Issue: Wrong Gradle version

**Error:**
```
This build requires Gradle 7.0 or higher
```

**Solution:**
Update Gradle installation:
```bash
gradle --version
# If < 7.0, update Gradle
```

### Issue: Groovy syntax error

**Error:**
```
Could not compile build file 'build.gradle'
```

**Solution:**
Check Groovy syntax:
- Use single quotes for strings
- Check closure syntax
- Verify method calls

## Documentation

For more information, see:

- [README.md](README.md) - Project overview
- [.gradle-docs/README.md](.gradle-docs/README.md) - Build documentation
- [.gradle-docs/CONFIGURATION.md](.gradle-docs/CONFIGURATION.md) - Configuration guide
- [MIGRATION-SUMMARY.md](MIGRATION-SUMMARY.md) - Migration details

## External Resources

- [Gradle Documentation](https://docs.gradle.org/)
- [Groovy DSL Reference](https://docs.gradle.org/current/dsl/)
- [Gradle User Manual](https://docs.gradle.org/current/userguide/userguide.html)
- [Groovy Language](https://groovy-lang.org/)

---

**Last Updated:** 2025-08-20  
**Version:** 2025.8.20  
**Build System:** Gradle with Groovy DSL (no wrapper)  
**Maintainer:** Bearsampp Team
