<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-memcached.svg?style=flat-square)](https://github.com/bearsampp/module-memcached/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-memcached/total.svg?style=flat-square)

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving Memcached.

## Documentation and downloads

https://bearsampp.com/module/memcached

## Building

This module uses Gradle for building releases. The build system provides modern features including:
- Native Gradle build (no Ant dependency)
- Automatic hash file generation (MD5, SHA1, SHA256, SHA512)
- Integration with modules-untouched repository
- Support for both bin/ and bin/archived/ directories
- Batch building of all versions

### Prerequisites

- Java 8 or higher
- Gradle (wrapper included)
- 7-Zip (for .7z format archives)

### Quick Start

```bash
# Display build information
gradle info

# Verify build environment
gradle verify

# List available versions in bin/ directory
gradle listVersions

# Build release for specific version (non-interactive)
gradle release -PbundleVersion=1.6.29

# Build release interactively (prompts for version)
gradle release

# Build all available versions
gradle releaseAll

# Clean build artifacts
gradle clean
```

### Available Tasks

**Build Tasks:**
- `gradle release -PbundleVersion=X.X.X` - Build release package for specific version
- `gradle releaseAll` - Build releases for all available versions
- `gradle clean` - Clean build artifacts

**Help Tasks:**
- `gradle info` - Display build configuration information
- `gradle tasks` - List all available tasks
- `gradle listVersions` - List available versions in bin/ and bin/archived/
- `gradle listReleases` - List available releases from modules-untouched

**Verification Tasks:**
- `gradle verify` - Verify build environment and dependencies
- `gradle validateProperties` - Validate build.properties configuration
- `gradle checkModulesUntouched` - Check modules-untouched integration

### Build Configuration

Build settings are configured in `build.properties`:

```properties
bundle.name = memcached
bundle.release = 2025.8.20
bundle.type = bins
bundle.format = 7z
```

### Output

Built releases are placed in:
- Build directory: `C:/Bearsampp-build/memcachedX.X.X/`
- Archive format: `.7z` (configurable)
- Hash files: `.md5`, `.sha1`, `.sha256`, `.sha512`

Archive structure:
- Each archive contains a top-level version folder, matching Bearsampp’s historical packaging (e.g., `memcached1.6.39/` with all files inside).

## Issues

Issues must be reported on [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).
