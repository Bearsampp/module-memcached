# Design Decisions - Bearsampp Module Memcached

## Overview

This document explains the design decisions made for the Bearsampp Module Memcached build system.

## Bundle Version Approach

### Why Use `-PbundleVersion`?

The build system uses `-PbundleVersion=X.X.X` instead of just `-Pversion=X.X.X` for the following reasons:

#### 1. Clarity and Specificity

| Approach                  | Clarity                                          |
|--------------------------|--------------------------------------------------|
| `-Pversion=1.6.39`       | Ambiguous - could mean project version or bundle |
| `-PbundleVersion=1.6.39` | Clear - explicitly refers to bundle version      |

**Rationale:** The term "bundleVersion" makes it immediately clear that we're referring to the Memcached version being packaged, not the build system version or release date.

#### 2. Separation of Concerns

The build system distinguishes between:

| Property          | Purpose                                          | Example      |
|------------------|--------------------------------------------------|--------------|
| `bundle.release` | Build/release date (from build.properties)       | `2025.8.20`  |
| `bundleVersion`  | Memcached version being packaged                 | `1.6.39`     |
| `project.version`| Gradle project version (same as bundle.release)  | `2025.8.20`  |

**Example:**
```bash
# Building Memcached 1.6.39 with release date 2025.8.20
gradle release -PbundleVersion=1.6.39

# Results in: bearsampp-memcached-1.6.39-2025.8.20.7z
```

#### 3. Consistency with Bearsampp Ecosystem

The Bearsampp project manages multiple software versions:

```
bin/
├── memcached1.6.15/    # Bundle version 1.6.15
├── memcached1.6.39/    # Bundle version 1.6.39
└── ...
```

Using `bundleVersion` makes it clear we're selecting which bundle to package.

#### 4. Avoids Gradle Conflicts

Gradle has a built-in `version` property for projects. Using `bundleVersion` avoids potential conflicts:

```groovy
// Project version (release date)
version = '2025.8.20'

// Bundle version (Memcached version) - separate property
def bundleVersion = project.findProperty('bundleVersion')
```

### Alternative Considered: `-Pversion`

**Why not used:**
- Conflicts with Gradle's project version
- Less explicit about what version is being specified
- Could cause confusion in multi-version scenarios

## Directory Display: `[bin]` Location Indicator

### Why Show `[bin]` Next to Versions?

The `listVersions` task displays directory locations like this:

```
Available memcached versions:
------------------------------------------------------------
  1.6.15          [bin]
  1.6.17          [bin]
  1.6.39          [bin]
------------------------------------------------------------
```

#### 1. Consistency with Other Modules

**Bruno Module Example:**
```
Available bruno versions:
------------------------------------------------------------
  2.13.0          [bin]
  2.12.0          [bin/archived]
------------------------------------------------------------
```

**Rationale:** Maintains consistency across all Bearsampp modules.

#### 2. Future Extensibility

The `[bin]` indicator allows for future directory structures:

| Location          | Purpose                                          |
|------------------|--------------------------------------------------|
| `[bin]`          | Active/current versions                          |
| `[bin/archived]` | Archived/old versions (future)                   |
| `[bin/beta]`     | Beta/testing versions (future)                   |

**Example Future Structure:**
```
bin/
├── memcached1.6.39/        # [bin]
├── memcached1.6.40/        # [bin]
└── archived/
    ├── memcached1.6.15/    # [bin/archived]
    └── memcached1.6.17/    # [bin/archived]
```

#### 3. Visual Clarity

The location indicator provides immediate visual feedback:

```
  1.6.39          [bin]           ← Active version
  1.6.15          [bin/archived]  ← Archived version
```

Users can quickly see where each version is located without navigating directories.

#### 4. Supports Multiple Storage Locations

Some modules (like Bruno) download and extract versions dynamically, while others (like Memcached) have pre-existing binaries. The location indicator works for both:

**Dynamic Download (Bruno):**
```groovy
// Downloads to bin/ or bin/archived/
def location = file("${binDir}/${bundleName}${version}").exists() ? "[bin]" : "[bin/archived]"
```

**Pre-existing Binaries (Memcached):**
```groovy
// All in bin/ currently
def location = "[bin]"
```

### Alternative Considered: No Location Indicator

**Why not used:**
- Less informative output
- Inconsistent with other modules
- Harder to extend for future directory structures
- Users would need to manually check directories

## Comparison with Other Modules

### Bruno Module

**Similarities:**
- ✓ Uses Groovy DSL
- ✓ No Gradle wrapper
- ✓ Shows `[bin]` location indicator
- ✓ Uses `-PbundleVersion` parameter

**Differences:**
- Bruno downloads and extracts versions dynamically
- Memcached uses pre-existing binaries in `bin/`

### Apache Module

**Similarities:**
- ✓ Pre-existing binaries in `bin/`
- ✓ Uses bundle versioning approach

**Differences:**
- May use different parameter names (to be standardized)

## Build System Architecture

### Why Pure Gradle?

| Aspect                | Ant/Gradle Hybrid      | Pure Gradle            |
|----------------------|------------------------|------------------------|
| Complexity           | High (two systems)     | Low (one system)       |
| Maintainability      | Difficult              | Easy                   |
| Performance          | Slower                 | Faster                 |
| IDE Support          | Limited                | Excellent              |
| Documentation        | Scattered              | Centralized            |

### Why Groovy DSL?

| Aspect                | Groovy DSL             | Kotlin DSL             |
|----------------------|------------------------|------------------------|
| Maturity             | Very mature            | Newer                  |
| Syntax               | Flexible, dynamic      | Strict, type-safe      |
| Build Speed          | Fast                   | Slower (compilation)   |
| Learning Curve       | Easier                 | Steeper                |
| Compatibility        | All Gradle versions    | Gradle 5.0+            |

### Why No Wrapper?

| Aspect                | With Wrapper           | Without Wrapper        |
|----------------------|------------------------|------------------------|
| Version Control      | Wrapper in repo        | Clean repo             |
| Flexibility          | Fixed Gradle version   | Use any Gradle version |
| Updates              | Manual wrapper update  | System-wide update     |
| Disk Space           | ~60MB per project      | Shared installation    |

**Rationale:** For a development environment like Bearsampp, developers already have Gradle installed system-wide. Using the wrapper would add unnecessary files to the repository.

## Property Naming Conventions

### Current Convention

| Property          | Source                | Purpose                           | Example      |
|------------------|----------------------|-----------------------------------|--------------|
| `bundleName`     | build.properties     | Software name                     | `memcached`  |
| `bundleRelease`  | build.properties     | Release date                      | `2025.8.20`  |
| `bundleVersion`  | Command line (-P)    | Software version to package       | `1.6.39`     |
| `bundleType`     | build.properties     | Bundle type                       | `bins`       |
| `bundleFormat`   | build.properties     | Archive format                    | `7z`         |

### Why "bundle" Prefix?

**Rationale:**
- Groups related properties together
- Distinguishes from Gradle built-in properties
- Makes purpose immediately clear
- Consistent across all Bearsampp modules

### Alternative Considered: No Prefix

**Example:**
```properties
name = memcached
release = 2025.8.20
type = bins
format = 7z
```

**Why not used:**
- Could conflict with Gradle properties
- Less clear about purpose
- Harder to grep/search in large projects

## Directory Structure

### Current Structure

```
module-memcached/
├── bin/                          # Pre-existing binaries
│   ├── memcached1.6.15/
│   ├── memcached1.6.39/
│   └── ...
├── .gradle-docs/                 # Build documentation
├── build.gradle                  # Groovy DSL build script
├── settings.gradle               # Gradle settings
├── gradle.properties             # Gradle configuration
├── build.properties              # Bundle configuration
└── releases.properties           # Release history
```

### Why `bin/` for Binaries?

**Rationale:**
- Standard convention across Bearsampp modules
- Clear purpose (binary files)
- Easy to .gitignore if needed
- Consistent with Unix/Linux conventions

### Why `.gradle-docs/` for Documentation?

**Rationale:**
- Hidden directory (starts with `.`)
- Clearly Gradle-related
- Doesn't clutter main directory
- Easy to find and navigate

## Release Naming Convention

### Current Format

```
bearsampp-{bundleName}-{bundleVersion}-{bundleRelease}.{bundleFormat}
```

**Example:**
```
bearsampp-memcached-1.6.39-2025.8.20.7z
```

### Why This Format?

| Component         | Purpose                                          | Example      |
|------------------|--------------------------------------------------|--------------|
| `bearsampp-`     | Project identifier                               | `bearsampp-` |
| `{bundleName}`   | Software name                                    | `memcached`  |
| `{bundleVersion}`| Software version                                 | `1.6.39`     |
| `{bundleRelease}`| Release/build date                               | `2025.8.20`  |
| `.{bundleFormat}`| Archive format                                   | `.7z`        |

**Benefits:**
- Immediately identifies project (Bearsampp)
- Shows what software is packaged
- Shows software version
- Shows when it was packaged
- Shows archive format

### Alternative Considered: Shorter Names

**Example:**
```
memcached-1.6.39.7z
```

**Why not used:**
- Doesn't identify Bearsampp project
- Missing release date information
- Less informative for users

## Future Considerations

### Potential Enhancements

1. **Archived Versions Support**
   ```
   bin/
   ├── memcached1.6.39/        # Current
   └── archived/
       └── memcached1.6.15/    # Old
   ```

2. **Beta/Testing Versions**
   ```
   bin/
   ├── memcached1.6.39/        # Stable
   └── beta/
       └── memcached1.6.40/    # Beta
   ```

3. **Automatic Version Detection**
   ```bash
   # Auto-detect latest version
   gradle release
   # Builds latest version automatically
   ```

4. **Multi-Version Builds**
   ```bash
   # Build multiple versions at once
   gradle releaseAll
   ```

## Conclusion

The design decisions made for the Bearsampp Module Memcached build system prioritize:

1. **Clarity** - Clear, explicit naming conventions
2. **Consistency** - Aligned with other Bearsampp modules
3. **Maintainability** - Pure Gradle, well-documented
4. **Extensibility** - Easy to add new features
5. **User Experience** - Informative output, easy to use

These decisions create a robust, maintainable build system that serves both current needs and future enhancements.

---

**Last Updated:** 2025-08-20  
**Version:** 2025.8.20  
**Maintainer:** Bearsampp Team
