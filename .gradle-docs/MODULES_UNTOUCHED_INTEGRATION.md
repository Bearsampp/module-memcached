# Modules-Untouched Integration

## Overview

The build system integrates with the [Bearsampp modules-untouched repository](https://github.com/Bearsampp/modules-untouched) to fetch version information and download URLs for Memcached releases.

## How It Works

### 1. Remote Properties File

The system fetches version information from:
```
https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/memcached.properties
```

This file contains mappings of version numbers to download URLs:
```properties
1.6.15 = https://example.com/memcached-1.6.15.zip
1.6.17 = https://example.com/memcached-1.6.17.zip
...
```

### 2. Version Resolution Strategy

The build system uses a two-tier strategy:

1. **Primary**: Fetch from modules-untouched repository
   - Attempts to download `memcached.properties` from GitHub
   - Parses version-to-URL mappings
   - Uses these URLs for downloads

2. **Fallback**: Standard URL construction
   - If remote fetch fails (network issues, file not found, etc.)
   - Constructs URLs using standard format
   - Continues build process without interruption

### 3. Implementation

The integration is implemented in the `fetchModulesUntouchedProperties()` helper function:

```groovy
def fetchModulesUntouchedProperties() {
    def propsUrl = "https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/memcached.properties"
    try {
        def connection = new URL(propsUrl).openConnection()
        connection.setRequestProperty("User-Agent", "Bearsampp-Build")
        connection.setConnectTimeout(5000)
        connection.setReadTimeout(5000)

        def props = new Properties()
        connection.inputStream.withCloseable { stream ->
            props.load(stream)
        }
        return props
    } catch (Exception e) {
        println "Warning: Could not fetch modules-untouched properties: ${e.message}"
        return null
    }
}
```

## Available Commands

### Check Integration Status

```bash
gradle checkModulesUntouched
```

This command:
- Tests connectivity to modules-untouched repository
- Displays all available versions
- Shows version resolution strategy
- Reports success or failure with details

### List Remote Releases

```bash
gradle listReleases
```

This command:
- Fetches the properties file from modules-untouched
- Lists all available versions and their URLs
- Shows total number of releases

## Benefits

### 1. Centralized Version Management

- Single source of truth for version information
- Easy to add new versions without modifying build scripts
- Consistent across all Bearsampp modules

### 2. Flexible URL Management

- URLs can be updated without changing build scripts
- Support for different download sources
- Easy migration to new hosting

### 3. Graceful Degradation

- Build continues even if remote fetch fails
- No hard dependency on network connectivity
- Fallback ensures builds always work

### 4. Version Discovery

- Automatically discover new versions
- No manual version list maintenance
- Easy to see what's available

## Error Handling

### Network Failures

If the remote fetch fails:
```
Warning: Could not fetch modules-untouched properties: Connection timeout
```

The build continues using fallback URL construction.

### File Not Found

If the properties file doesn't exist:
```
Warning: Could not fetch modules-untouched properties: 404 Not Found
```

The build continues with fallback strategy.

### Parse Errors

If the properties file is malformed:
```
Warning: Could not fetch modules-untouched properties: Invalid properties format
```

The build continues with fallback strategy.

## Configuration

### Timeout Settings

The integration uses conservative timeout settings:
- **Connect Timeout**: 5 seconds
- **Read Timeout**: 5 seconds

These can be adjusted in the `fetchModulesUntouchedProperties()` function if needed.

### User Agent

The system identifies itself as `Bearsampp-Build` in HTTP requests.

## Testing

### Test Integration

```bash
gradle checkModulesUntouched
```

Expected output:
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
  1.6.15    
  1.6.17    
  ...
======================================================================
Total versions: 12

======================================================================
[SUCCESS] modules-untouched integration is working
======================================================================
```

### Test Fallback

To test the fallback mechanism:
1. Disconnect from the internet
2. Run `gradle checkModulesUntouched`
3. Observe the warning message and fallback behavior

## Maintenance

### Adding New Versions

To add a new Memcached version:

1. Update the `memcached.properties` file in modules-untouched repository
2. Add the version-to-URL mapping
3. Commit and push changes
4. The build system will automatically pick up the new version

### Updating URLs

To update download URLs:

1. Edit the `memcached.properties` file in modules-untouched repository
2. Update the URL for the specific version
3. Commit and push changes
4. No build script changes needed

## Related Tasks

- `gradle listReleases` - List all remote releases
- `gradle checkModulesUntouched` - Test integration
- `gradle listVersions` - List local versions
- `gradle verify` - Verify build environment
