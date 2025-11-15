# Bearsampp Module Memcached - Gradle Build Documentation

This directory contains documentation for the Gradle build system used in the Bearsampp Memcached module.

## Documentation Files

- **QUICK_REFERENCE.md** - Quick reference guide for common tasks
- **INTERACTIVE_MODE.md** - Interactive vs non-interactive build modes
- **FEATURE_SUMMARY.md** - Overview of build system features
- **MODULES_UNTOUCHED_INTEGRATION.md** - Integration with modules-untouched repository
- **CONFIGURATION_SUMMARY.md** - Build configuration details
- **MIGRATION_SUMMARY.md** - Migration from Ant to Gradle details

## Quick Start

```bash
# Display build information
gradle info

# Verify build environment
gradle verify

# List available versions
gradle listVersions

# Build a specific version
gradle release -PbundleVersion=1.6.29

# Build all versions
gradle releaseAll
```

## Key Features

- **Native Gradle Build**: No Ant dependency, pure Gradle implementation
- **Hash File Generation**: Automatic MD5, SHA1, SHA256, SHA512 hash files
- **Modules-Untouched Integration**: Fetches version information from remote repository
- **Batch Building**: Build all available versions with a single command
- **Archive Support**: 7z and zip format support
- **Version Management**: Support for both bin/ and bin/archived/ directories

## Build System Architecture

The build system is organized into several key components:

1. **Configuration** (`build.properties`)
   - Bundle name, version, type, and format
   - Build paths and output directories

2. **Helper Functions**
   - `fetchModulesUntouchedProperties()` - Fetch version info from remote
   - `find7ZipExecutable()` - Locate 7-Zip installation
   - `generateHashFiles()` - Create hash files for archives
   - `calculateHash()` - Calculate file hashes
   - `getAvailableVersions()` - List available versions

3. **Build Tasks**
   - `release` - Build single version
   - `releaseAll` - Build all versions
   - `clean` - Clean build artifacts

4. **Verification Tasks**
   - `verify` - Check build environment
   - `validateProperties` - Validate configuration
   - `checkModulesUntouched` - Test remote integration

5. **Help Tasks**
   - `info` - Display build information
   - `listVersions` - List local versions
   - `listReleases` - List remote versions

## Requirements

- Java 8 or higher
- Gradle (wrapper included)
- 7-Zip (for .7z format archives)
- Internet connection (for modules-untouched integration)

## Support

For issues and questions, please refer to the main [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).
