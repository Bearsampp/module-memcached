# Migration Guide: Ant to Pure Gradle

Guide for migrating from the Ant-based build system to the pure Gradle build system.

## Table of Contents

- [Overview](#overview)
- [What Changed](#what-changed)
- [Command Mapping](#command-mapping)
- [Configuration Changes](#configuration-changes)
- [Benefits](#benefits)
- [Migration Steps](#migration-steps)
- [Troubleshooting](#troubleshooting)

## Overview

The Bearsampp Module Memcached build system has been migrated from a hybrid Ant/Gradle system to a pure Gradle build system.

### Migration Summary

| Aspect                | Before (Ant/Gradle)                    | After (Pure Gradle)                    |
|----------------------|----------------------------------------|----------------------------------------|
| Build System         | Hybrid Ant + Gradle                    | Pure Gradle                            |
| Build Files          | `build.xml`, `build.gradle`            | `build.gradle` only                    |
| Task Execution       | Ant tasks via Gradle wrapper           | Native Gradle tasks                    |
| Dependencies         | External Ant build files               | Self-contained                         |
| Documentation        | Scattered                              | Centralized in `.gradle-docs/`         |

## What Changed

### Removed

| Item                          | Reason                                           |
|------------------------------|--------------------------------------------------|
| `build.xml`                  | Replaced by native Gradle tasks                  |
| Ant task imports             | No longer needed                                 |
| External Ant dependencies    | Build is now self-contained                      |
| Ant-specific properties      | Converted to Gradle properties                   |

### Added

| Item                          | Purpose                                          |
|------------------------------|--------------------------------------------------|
| Pure Gradle tasks            | Native Gradle implementation                     |
| `.gradle-docs/`              | Comprehensive documentation                      |
| Enhanced output formatting   | Better user experience                           |
| Build verification           | Environment checking                             |

### Modified

| Item                          | Change                                           |
|------------------------------|--------------------------------------------------|
| `build.gradle`               | Removed Ant imports, added native tasks          |
| `settings.gradle`            | Removed Ant references                           |
| `gradle.properties`          | Added performance optimizations                  |
| `README.md`                  | Updated with new documentation structure         |

## Command Mapping

### Build Commands

| Old Command (Ant)                          | New Command (Gradle)                              |
|-------------------------------------------|---------------------------------------------------|
| `ant release -Dinput.bundle=1.6.39`       | `gradle release -PbundleVersion=1.6.39`           |
| `ant clean`                               | `gradle clean`                                    |
| `ant -projecthelp`                        | `gradle tasks`                                    |

### Information Commands

| Old Command (Ant)                          | New Command (Gradle)                              |
|-------------------------------------------|---------------------------------------------------|
| N/A                                       | `gradle info`                                     |
| N/A                                       | `gradle listVersions`                             |
| N/A                                       | `gradle listReleases`                             |
| N/A                                       | `gradle verify`                                   |
| N/A                                       | `gradle validateProperties`                       |

### Interactive vs Non-Interactive

**Old (Ant):**
```bash
# Interactive only
ant release
# Prompted for version input
```

**New (Gradle):**
```bash
# Interactive - shows available versions
gradle release

# Non-interactive - builds specific version
gradle release -PbundleVersion=1.6.39
```

## Configuration Changes

### build.properties

**No changes required** - The file format remains the same:

```properties
bundle.name = memcached
bundle.release = 2025.8.20
bundle.type = bins
bundle.format = 7z
```

### gradle.properties

**Enhanced** with performance optimizations:

```properties
# Old (minimal)
# (empty or basic settings)

# New (optimized)
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true
org.gradle.configureondemand=true
org.gradle.jvmargs=-Xmx2048m -Xms512m -XX:MaxMetaspaceSize=512m
org.gradle.console=rich
org.gradle.warning.mode=all
org.gradle.vfs.watch=true
```

### releases.properties

**No changes required** - The file format remains the same.

## Benefits

### Performance

| Benefit                       | Impact                                           |
|------------------------------|--------------------------------------------------|
| Native Gradle tasks          | Faster execution                                 |
| Build caching                | Reuse previous build outputs                     |
| Parallel execution           | Better multi-core utilization                    |
| Daemon mode                  | Reduced startup time                             |
| Incremental builds           | Only rebuild what changed                        |

### Maintainability

| Benefit                       | Impact                                           |
|------------------------------|--------------------------------------------------|
| Pure Gradle                  | Simpler codebase                                 |
| Self-contained               | No external dependencies                         |
| Better documentation         | Easier to understand and modify                  |
| Modern tooling               | Better IDE support                               |

### User Experience

| Benefit                       | Impact                                           |
|------------------------------|--------------------------------------------------|
| Better output formatting     | Clearer feedback                                 |
| More informative messages    | Easier troubleshooting                           |
| Comprehensive help           | Better discoverability                           |
| Verification tasks           | Catch issues early                               |

## Migration Steps

### For Users

If you're using the build system, follow these steps:

#### Step 1: Update Repository

```bash
# Pull latest changes
git pull origin main

# Verify files
dir
```

**Expected files:**
- `build.gradle` (updated)
- `settings.gradle` (updated)
- `gradle.properties` (updated)
- `.gradle-docs/` (new)
- `build.xml` (removed)

#### Step 2: Verify Environment

```bash
# Verify build environment
gradle verify
```

**Expected output:**
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

#### Step 3: Update Commands

Replace old Ant commands with new Gradle commands:

**Old:**
```bash
ant release -Dinput.bundle=1.6.39
```

**New:**
```bash
gradle release -PbundleVersion=1.6.39
```

#### Step 4: Learn New Features

Explore new tasks:

```bash
# Display build information
gradle info

# List available versions
gradle listVersions

# List all releases
gradle listReleases

# List all tasks
gradle tasks
```

#### Step 5: Read Documentation

Read the new documentation:

```bash
# Open documentation
start .gradle-docs/README.md
```

Or browse online:
- [README.md](.gradle-docs/README.md)
- [TASKS.md](.gradle-docs/TASKS.md)
- [CONFIGURATION.md](.gradle-docs/CONFIGURATION.md)
- [RELEASE-PROCESS.md](.gradle-docs/RELEASE-PROCESS.md)

### For Developers

If you're modifying the build system:

#### Step 1: Understand New Structure

Review the new build structure:

```
module-memcached/
├── build.gradle              # Pure Gradle build script
├── settings.gradle           # Gradle settings
├── gradle.properties         # Gradle configuration
├── build.properties          # Bundle configuration
├── releases.properties       # Release history
└── .gradle-docs/             # Documentation
    ├── README.md
    ├── TASKS.md
    ├── CONFIGURATION.md
    ├── RELEASE-PROCESS.md
    ├── MIGRATION-GUIDE.md
    └── INDEX.md
```

#### Step 2: Review Build Script

Study the new `build.gradle`:

- Pure Gradle implementation
- No Ant imports
- Native task definitions
- Enhanced output formatting
- Comprehensive error handling

#### Step 3: Test Changes

Test your changes:

```bash
# Verify environment
gradle verify

# Test build
gradle release -PbundleVersion=1.6.39

# Clean up
gradle clean
```

#### Step 4: Update Documentation

Update documentation if needed:

- Update relevant `.gradle-docs/*.md` files
- Update version and date
- Test all examples
- Update index if structure changes

### For CI/CD

If you're using CI/CD pipelines:

#### Step 1: Update Pipeline Scripts

**Old (Ant):**
```yaml
- name: Build Release
  run: ant release -Dinput.bundle=1.6.39
```

**New (Gradle):**
```yaml
- name: Build Release
  run: gradle release -PbundleVersion=1.6.39
```

#### Step 2: Update Environment

Ensure CI/CD environment has:

- Java 8+
- Gradle 7.0+
- 7-Zip (for 7z format)

#### Step 3: Update Caching

Leverage Gradle caching:

```yaml
- name: Cache Gradle
  uses: actions/cache@v3
  with:
    path: |
      ~/.gradle/caches
      ~/.gradle/wrapper
    key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*') }}
```

#### Step 4: Test Pipeline

Test the updated pipeline:

```bash
# Run pipeline locally (if possible)
# Or trigger test build on CI/CD
```

## Troubleshooting

### Common Issues

#### Issue: build.xml not found

**Error:**
```
build.xml not found
```

**Cause:** Trying to use old Ant commands

**Solution:** Use new Gradle commands:
```bash
# Old
ant release -Dinput.bundle=1.6.39

# New
gradle release -PbundleVersion=1.6.39
```

---

#### Issue: Task not found

**Error:**
```
Task 'ant-release' not found
```

**Cause:** Trying to use old Ant task names

**Solution:** Use new Gradle task names:
```bash
# List available tasks
gradle tasks

# Use correct task name
gradle release -PbundleVersion=1.6.39
```

---

#### Issue: Property not recognized

**Error:**
```
Property 'input.bundle' not recognized
```

**Cause:** Using old Ant property names

**Solution:** Use new Gradle property names:
```bash
# Old
-Dinput.bundle=1.6.39

# New
-PbundleVersion=1.6.39
```

---

#### Issue: Build fails with "dev path not found"

**Error:**
```
Dev path not found: E:/Bearsampp-development/dev
```

**Cause:** Old build script looking for external Ant files

**Solution:** Update to latest build.gradle:
```bash
git pull origin main
```

---

### Getting Help

If you encounter issues:

1. **Read Documentation**
   - [README.md](.gradle-docs/README.md)
   - [TASKS.md](.gradle-docs/TASKS.md)
   - [CONFIGURATION.md](.gradle-docs/CONFIGURATION.md)

2. **Check Troubleshooting**
   - [README.md - Troubleshooting](.gradle-docs/README.md#troubleshooting)
   - [CONFIGURATION.md - Troubleshooting](.gradle-docs/CONFIGURATION.md#troubleshooting-configuration)
   - [RELEASE-PROCESS.md - Troubleshooting](.gradle-docs/RELEASE-PROCESS.md#troubleshooting)

3. **Verify Environment**
   ```bash
   gradle verify
   ```

4. **Run with Debug**
   ```bash
   gradle release -PbundleVersion=1.6.39 --info
   gradle release -PbundleVersion=1.6.39 --debug
   ```

5. **Report Issues**
   - [GitHub Issues](https://github.com/bearsampp/bearsampp/issues)

## Comparison

### Side-by-Side Comparison

#### Building a Release

**Old (Ant):**
```bash
# Interactive only
ant release
# Enter version when prompted: 1.6.39

# Or with property
ant release -Dinput.bundle=1.6.39
```

**New (Gradle):**
```bash
# Interactive - shows available versions
gradle release

# Non-interactive - builds specific version
gradle release -PbundleVersion=1.6.39
```

#### Listing Information

**Old (Ant):**
```bash
# No built-in commands
# Manual inspection required
dir bin
type releases.properties
```

**New (Gradle):**
```bash
# List available versions
gradle listVersions

# List all releases
gradle listReleases

# Display build info
gradle info
```

#### Verification

**Old (Ant):**
```bash
# No built-in verification
# Manual checks required
```

**New (Gradle):**
```bash
# Verify environment
gradle verify

# Validate properties
gradle validateProperties
```

### Feature Comparison

| Feature                       | Ant/Gradle Hybrid      | Pure Gradle            |
|------------------------------|------------------------|------------------------|
| Build Speed                  | Moderate               | Fast                   |
| Caching                      | Limited                | Full                   |
| Parallel Execution           | No                     | Yes                    |
| Incremental Builds           | No                     | Yes                    |
| Self-Contained               | No                     | Yes                    |
| Documentation                | Basic                  | Comprehensive          |
| Verification                 | No                     | Yes                    |
| Output Formatting            | Basic                  | Enhanced               |
| Error Messages               | Basic                  | Detailed               |
| IDE Support                  | Limited                | Full                   |

## Best Practices

### After Migration

1. **Update Bookmarks**
   - Update any documentation bookmarks
   - Update command references
   - Update CI/CD scripts

2. **Train Team**
   - Share new documentation
   - Demonstrate new commands
   - Explain benefits

3. **Monitor**
   - Watch for issues
   - Gather feedback
   - Improve documentation

4. **Optimize**
   - Tune Gradle settings
   - Leverage caching
   - Use parallel execution

## Rollback

If you need to rollback to the old system:

```bash
# Checkout previous version
git checkout <previous-commit>

# Or revert changes
git revert <migration-commit>
```

**Note:** Rollback is not recommended as the new system provides significant improvements.

## Feedback

We welcome feedback on the migration:

- Report issues: [GitHub Issues](https://github.com/bearsampp/bearsampp/issues)
- Suggest improvements: [GitHub Discussions](https://github.com/bearsampp/bearsampp/discussions)
- Contribute: [Contributing Guide](../README.md#contributing)

---

**Last Updated:** 2025-08-20  
**Version:** 2025.8.20  
**Maintainer:** Bearsampp Team
