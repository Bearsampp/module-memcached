# Gradle Tasks Reference

Complete reference for all available Gradle tasks in the Bearsampp Module Memcached build system.

## Table of Contents

- [Build Tasks](#build-tasks)
- [Verification Tasks](#verification-tasks)
- [Help Tasks](#help-tasks)
- [Documentation Tasks](#documentation-tasks)
- [Task Dependencies](#task-dependencies)
- [Custom Properties](#custom-properties)

## Build Tasks

### release

Build a release package for a specific Memcached version.

**Group:** `build`

**Syntax:**

```bash
# Interactive mode (lists available versions)
gradle release

# Non-interactive mode (builds specific version)
gradle release -PbundleVersion=<version>
```

**Parameters:**

| Parameter          | Type     | Required | Description                           | Example      |
|-------------------|----------|----------|---------------------------------------|--------------|
| `bundleVersion`   | String   | No       | Version to build (e.g., "1.6.39")     | `1.6.39`     |

**Examples:**

```bash
# Interactive mode - shows available versions
gradle release

# Build version 1.6.39
gradle release -PbundleVersion=1.6.39

# Build version 1.6.38
gradle release -PbundleVersion=1.6.38
```

**Output:**

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

**Process:**

1. Validates the specified version exists in `bin/` directory
2. Creates temporary build directory
3. Copies bundle files to temporary location
4. Creates 7z archive with proper naming convention
5. Outputs archive location and size

**Error Handling:**

- **Version not found:** Lists available versions
- **Missing 7z:** Provides installation instructions
- **Permission errors:** Suggests running with elevated privileges

---

### clean

Clean build artifacts and temporary files.

**Group:** `build`

**Syntax:**

```bash
gradle clean
```

**Examples:**

```bash
# Clean all build artifacts
gradle clean

# Clean and rebuild
gradle clean release -PbundleVersion=1.6.39
```

**Output:**

```
Cleaned: E:\Bearsampp-development\module-memcached\build
Cleaned: C:\Users\troy\Bearsampp-build\tmp

[SUCCESS] Build artifacts cleaned
```

**Cleaned Directories:**

| Directory                                  | Description                    |
|-------------------------------------------|--------------------------------|
| `build/`                                  | Gradle build directory         |
| `${buildPath}/tmp/`                       | Temporary build files          |

---

## Verification Tasks

### verify

Verify the build environment and dependencies.

**Group:** `verification`

**Syntax:**

```bash
gradle verify
```

**Examples:**

```bash
# Verify build environment
gradle verify

# Verify with detailed output
gradle verify --info
```

**Output:**

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
  �� PASS     bin/ directory
  ✓ PASS     7z command
────────────────────────────────────────────────────────────────────────────

[SUCCESS] All checks passed! Build environment is ready.

You can now run:
  gradle release -PbundleVersion=1.6.39     - Build specific version
  gradle listVersions                        - List available versions
```

**Checks Performed:**

| Check                  | Description                                    | Requirement      |
|-----------------------|------------------------------------------------|------------------|
| Java 8+               | Java version 8 or higher                       | Required         |
| build.gradle          | Main build script exists                       | Required         |
| build.properties      | Bundle configuration exists                    | Required         |
| releases.properties   | Release history exists                         | Required         |
| settings.gradle       | Gradle settings exists                         | Required         |
| bin/ directory        | Binary directory exists                        | Required         |
| 7z command            | 7-Zip command available                        | Required         |

---

### validateProperties

Validate build.properties configuration.

**Group:** `verification`

**Syntax:**

```bash
gradle validateProperties
```

**Examples:**

```bash
# Validate properties
gradle validateProperties
```

**Output:**

```
╔════════════════════════════════════════════════════════════════════════════╗
║                    Validating build.properties                             ║
╚════════════════════════════════════════════════════════════════════════════╝

[SUCCESS] All required properties are present:

Property             Value
──────────────────────────────────────────────────
bundle.name          memcached
bundle.release       2025.8.20
bundle.type          bins
bundle.format        7z
──────────────────────────────────────────────────
```

**Validated Properties:**

| Property          | Required | Description                           |
|------------------|----------|---------------------------------------|
| `bundle.name`    | Yes      | Bundle name (e.g., "memcached")       |
| `bundle.release` | Yes      | Release date (e.g., "2025.8.20")      |
| `bundle.type`    | Yes      | Bundle type (e.g., "bins")            |
| `bundle.format`  | Yes      | Archive format (e.g., "7z")           |

---

## Help Tasks

### info

Display build configuration information.

**Group:** `help`

**Syntax:**

```bash
gradle info
```

**Examples:**

```bash
# Display build information
gradle info
```

**Output:**

```
╔════════════════════════════════════════════════════════════════════════════╗
║                Bearsampp Module Memcached - Build Info                     ║
╚════════════════════════════════════════════════════════════════════════════╝

Project Information:
  Name:                 module-memcached
  Version:              2025.8.20
  Description:          Bearsampp Module - memcached
  Group:                com.bearsampp.modules

Bundle Properties:
  Name:                 memcached
  Release:              2025.8.20
  Type:                 bins
  Format:               7z

Paths:
  Project Directory:    E:/Bearsampp-development/module-memcached
  Root Directory:       E:/Bearsampp-development
  Build Path:           C:\Users\troy\Bearsampp-build
  Temp Path:            C:\Users\troy\Bearsampp-build\tmp
  Release Path:         C:\Users\troy\Bearsampp-build\release

Environment:
  Java Version:         17.0.2
  Java Home:            C:\Program Files\Java\jdk-17.0.2
  Gradle Version:       8.5
  Gradle Home:          C:\Users\troy\.gradle\wrapper\dists\gradle-8.5
  Operating System:     Windows 11 10.0

Available Task Groups:
  • build               - Build and package tasks
  • verification        - Verification and validation tasks
  • help                - Help and information tasks
  • documentation       - Documentation tasks

Quick Start:
  gradle tasks                              - List all available tasks
  gradle info                               - Show this information
  gradle release                            - Interactive release build
  gradle release -PbundleVersion=1.6.39     - Non-interactive release
  gradle clean                              - Clean build artifacts
  gradle verify                             - Verify build environment
  gradle listVersions                       - List available bundle versions
  gradle listReleases                       - List all releases

Documentation:
  See .gradle-docs/ for comprehensive build documentation
```

---

### listVersions

List all available bundle versions in the bin/ directory.

**Group:** `help`

**Syntax:**

```bash
gradle listVersions
```

**Examples:**

```bash
# List available versions
gradle listVersions
```

**Output:**

```
╔════════════════════════════════════════════════════════════════════════════╗
║                  Available memcached Versions in bin/                      ║
╚════════════════════════════════════════════════════════════════════════════╝

Version         Size            Path
────────────────────────────────────────────────────────────────────────────
1.6.15          0.42 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.15
1.6.17          0.43 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.17
1.6.18          0.43 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.18
1.6.21          0.44 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.21
1.6.24          0.44 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.24
1.6.29          0.45 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.29
1.6.31          0.45 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.31
1.6.32          0.45 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.32
1.6.33          0.45 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.33
1.6.36          0.45 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.36
1.6.37          0.45 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.37
1.6.38          0.45 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.38
1.6.39          0.45 MB         E:/Bearsampp-development/module-memcached/bin/memcached1.6.39
─────────────────────────────────────────────────────���──────────────────────

Total versions: 13

To build a specific version:
  gradle release -PbundleVersion=1.6.39
```

---

### listReleases

List all available releases from releases.properties.

**Group:** `help`

**Syntax:**

```bash
gradle listReleases
```

**Examples:**

```bash
# List all releases
gradle listReleases
```

**Output:**

```
╔════════════════════════════════════════════════════════════════════════════╗
║                      Available Memcached Releases                          ║
╚════════════════════════════════════════════════════════════════════════════╝

Version      Download URL
────────────────────────────────────────────────────────────────────────────
1.6.6        https://github.com/Bearsampp/module-memcached/releases/download/2022.07.14/bearsampp-memcached-1.6.6-2022.07.14.7z
1.6.15       https://github.com/Bearsampp/module-memcached/releases/download/2022.08.04/bearsampp-memcached-1.6.15-2022.08.04.7z
1.6.17       https://github.com/Bearsampp/module-memcached/releases/download/2022.09.26/bearsampp-memcached-1.6.17-2022.09.24.7z
1.6.18       https://github.com/Bearsampp/module-memcached/releases/download/2023.3.5/bearsampp-memcached-1.6.18-2023.3.5.7z
1.6.21       https://github.com/Bearsampp/module-memcached/releases/download/2023.10.1/bearsampp-memcached-1.6.21-2023.10.1.7z
1.6.24       https://github.com/Bearsampp/module-memcached/releases/download/2024.3.30/bearsampp-memcached-1.6.24-2024.3.30.7z
1.6.29       https://github.com/Bearsampp/module-memcached/releases/download/2024.7.29/bearsampp-memcached-1.6.29-2024.10.7.7z
1.6.31       https://github.com/Bearsampp/module-memcached/releases/download/2024.10.7/bearsampp-memcached-1.6.31-2024.10.7.7z
1.6.32       https://github.com/Bearsampp/module-memcached/releases/download/2024.12.1/bearsampp-memcached-1.6.32-2024.12.1.7z
1.6.33       https://github.com/Bearsampp/module-memcached/releases/download/2024.12.23/bearsampp-memcached-1.6.33-2024.12.23.7z
1.6.36       https://github.com/Bearsampp/module-memcached/releases/download/2025.2.11/bearsampp-memcached-1.6.36-2025.2.11.7z
1.6.38       https://github.com/Bearsampp/module-memcached/releases/download/2025.4.19/bearsampp-memcached-1.6.38-2025.4.19.7z
1.6.39       https://github.com/Bearsampp/module-memcached/releases/download/2025.8.20/bearsampp-memcached-1.6.39-2025.8.20.7z
────────────────────────────────────────────────────────────────────────────

Total releases: 13
```

---

### tasks

List all available Gradle tasks.

**Group:** `help`

**Syntax:**

```bash
gradle tasks
gradle tasks --all
```

**Examples:**

```bash
# List main tasks
gradle tasks

# List all tasks including internal ones
gradle tasks --all
```

---

## Documentation Tasks

### generateDocs

Generate build documentation in .gradle-docs/.

**Group:** `documentation`

**Syntax:**

```bash
gradle generateDocs
```

**Examples:**

```bash
# Generate documentation
gradle generateDocs
```

**Output:**

```
Generating documentation in .gradle-docs/...
[SUCCESS] Documentation directory created
See .gradle-docs/ for comprehensive build documentation
```

---

## Task Dependencies

### Dependency Graph

```
release
  (no dependencies)

clean
  (no dependencies)

verify
  (no dependencies)

validateProperties
  (no dependencies)

info
  (no dependencies)

listVersions
  (no dependencies)

listReleases
  (no dependencies)

generateDocs
  (no dependencies)
```

### Common Task Chains

```bash
# Clean and build
gradle clean release -PbundleVersion=1.6.39

# Verify and build
gradle verify release -PbundleVersion=1.6.39

# Validate and build
gradle validateProperties release -PbundleVersion=1.6.39

# Full verification and build
gradle clean verify validateProperties release -PbundleVersion=1.6.39
```

---

## Custom Properties

### Project Properties

Properties that can be passed via command line:

| Property          | Type     | Description                           | Example                                      |
|------------------|----------|---------------------------------------|----------------------------------------------|
| `bundleVersion`  | String   | Version to build                      | `-PbundleVersion=1.6.39`                     |

### System Properties

System properties that can be set:

| Property                  | Type      | Description                      | Example                                  |
|--------------------------|-----------|----------------------------------|------------------------------------------|
| `org.gradle.daemon`      | Boolean   | Enable Gradle daemon             | `-Dorg.gradle.daemon=true`               |
| `org.gradle.parallel`    | Boolean   | Enable parallel execution        | `-Dorg.gradle.parallel=true`             |
| `org.gradle.caching`     | Boolean   | Enable build caching             | `-Dorg.gradle.caching=true`              |

### Environment Variables

Environment variables used by the build:

| Variable          | Description                           | Example                                      |
|------------------|---------------------------------------|----------------------------------------------|
| `JAVA_HOME`      | Java installation directory           | `C:\Program Files\Java\jdk-17.0.2`           |
| `GRADLE_HOME`    | Gradle installation directory         | `C:\Gradle\gradle-8.5`                       |
| `PATH`           | System path (must include 7z)         | `C:\Program Files\7-Zip;...`                 |

---

## Task Output Formats

### Success Output

```
╔════════════════════════════════════════════════════════════════════════════╗
║                      [Task Name] Completed                                 ║
╚════════════════════════════════════════════════════════════════════════════╝

[SUCCESS] Task completed successfully
```

### Error Output

```
╔════════════════════════════════════════════════════════════════════════════╗
║                      [Task Name] Failed                                    ║
╚════════════════════════════════════════════════════════════════════════════╝

[ERROR] Error message here

Possible solutions:
  • Solution 1
  • Solution 2
```

### Warning Output

```
[WARNING] Warning message here

Please review:
  • Item 1
  • Item 2
```

---

**Last Updated:** 2025-08-20  
**Version:** 2025.8.20  
**Maintainer:** Bearsampp Team
