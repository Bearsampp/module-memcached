# Quick Reference Guide

## Common Commands

### Build Commands

```bash
# Build a specific version (non-interactive)
gradle release -PbundleVersion=1.6.29

# Build interactively (prompts for version selection)
gradle release

# Build all available versions
gradle releaseAll

# Clean build artifacts
gradle clean
```

### Information Commands

```bash
# Display build information
gradle info

# List all available tasks
gradle tasks

# List versions in bin/ directory
gradle listVersions

# List releases from modules-untouched
gradle listReleases
```

### Verification Commands

```bash
# Verify build environment
gradle verify

# Validate build.properties
gradle validateProperties

# Check modules-untouched integration
gradle checkModulesUntouched
```

## Build Output

Built releases are placed in:
- **Build directory**: `C:/Bearsampp-build/memcachedX.X.X/`
- **Archive format**: `.7z` (configurable in build.properties)
- **Hash files**: `.md5`, `.sha1`, `.sha256`, `.sha512`

## Configuration

Edit `build.properties` to configure:

```properties
bundle.name = memcached
bundle.release = 2025.8.20
bundle.type = bins
bundle.format = 7z
#build.path = C:/Bearsampp-build
```

## Troubleshooting

### 7-Zip not found

If you get a "7-Zip executable not found" error:

1. Install 7-Zip from https://www.7-zip.org/
2. Or set the `7Z_HOME` environment variable to your 7-Zip installation directory

### Version not found

If a version is not found:

1. Run `gradle listVersions` to see available versions
2. Ensure the version exists in `bin/` or `bin/archived/` directory
3. Check the version format matches `memcachedX.X.X`

### Network issues

If modules-untouched integration fails:

1. Check your internet connection
2. The build will continue using fallback URL construction
3. Run `gradle checkModulesUntouched` to test the connection

## Tips

- Use `gradle tasks --all` to see all available tasks including internal ones
- Use `gradle help --task <taskname>` to get detailed help for a specific task
- Enable Gradle daemon for faster builds: `gradle --daemon`
- Use `--info` or `--debug` flags for more detailed output
