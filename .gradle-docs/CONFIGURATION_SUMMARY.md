# Configuration Summary

## Overview

The Bearsampp Memcached module build system is configured through the `build.properties` file and various Gradle project settings.

## build.properties

The main configuration file for the build system.

### Location
```
D:/Bearsampp-dev/module-memcached/build.properties
```

### Properties

#### bundle.name
- **Type**: String
- **Default**: `memcached`
- **Description**: The name of the bundle/module
- **Usage**: Used in directory names, archive names, and output paths

```properties
bundle.name = memcached
```

#### bundle.release
- **Type**: Version String
- **Default**: `2025.8.20`
- **Description**: The release version of the build configuration
- **Usage**: Project version, metadata

```properties
bundle.release = 2025.8.20
```

#### bundle.type
- **Type**: String
- **Default**: `bins`
- **Options**: `bins`, `tools`, `apps`
- **Description**: The type of bundle being built
- **Usage**: Categorization, metadata

```properties
bundle.type = bins
```

#### bundle.format
- **Type**: String
- **Default**: `7z`
- **Options**: `7z`, `zip`
- **Description**: Archive format for release packages
- **Usage**: Determines compression method and file extension

```properties
bundle.format = 7z
```

#### build.path (Optional)
- **Type**: Path String
- **Default**: `C:/Bearsampp-build`
- **Description**: Output directory for built releases
- **Usage**: Destination for release archives and hash files

```properties
#build.path = C:/Bearsampp-build
```

## Gradle Configuration

### Project Settings

Defined in `build.gradle`:

```groovy
group = 'com.bearsampp.modules'
version = buildProps.getProperty('bundle.release', '1.0.0')
description = "Bearsampp Module - ${buildProps.getProperty('bundle.name', 'memcached')}"
```

### Path Configuration

```groovy
ext {
    projectBasedir = projectDir.absolutePath
    rootDir = projectDir.parent
    devPath = file("${rootDir}/dev").absolutePath
    buildPropertiesFile = file('build.properties').absolutePath

    // Bundle properties from build.properties
    bundleName = buildProps.getProperty('bundle.name', 'memcached')
    bundleRelease = buildProps.getProperty('bundle.release', '1.0.0')
    bundleType = buildProps.getProperty('bundle.type', 'bins')
    bundleFormat = buildProps.getProperty('bundle.format', '7z')

    // Build paths
    bundleBuildPath = buildProps.getProperty('build.path', 'C:/Bearsampp-build')
    bundleTmpPrepPath = "${bundleBuildPath}/tmp/prep"
}
```

## Directory Structure

### Source Directories

```
D:/Bearsampp-dev/module-memcached/
├── bin/                          # Version directories
│   ├── memcached1.6.15/
│   ├── memcached1.6.17/
│   └── ...
└── bin/archived/                 # Archived versions
    ├── memcached1.6.10/
    └── ...
```

### Build Output Structure

```
C:/Bearsampp-build/
├── memcached1.6.29/
│   ├── memcached1.6.29.7z
│   ├── memcached1.6.29.7z.md5
│   ├── memcached1.6.29.7z.sha1
│   ├── memcached1.6.29.7z.sha256
│   └── memcached1.6.29.7z.sha512
└── tmp/
    └── prep/
        └── memcached1.6.29/      # Staging directory
```

## Environment Variables

### 7Z_HOME (Optional)

- **Description**: Path to 7-Zip installation directory
- **Usage**: Used to locate 7z.exe if not in standard locations
- **Example**: `C:/Program Files/7-Zip`

```bash
# Windows
set 7Z_HOME=C:\Program Files\7-Zip

# PowerShell
$env:7Z_HOME = "C:\Program Files\7-Zip"
```

## Task Configuration

### Default Task

```groovy
defaultTasks 'info'
```

Running `gradle` without arguments executes the `info` task.

### Task Groups

Tasks are organized into groups:

- **build**: Build and package tasks
- **help**: Information and help tasks
- **verification**: Validation and verification tasks

## Validation

### Required Properties

The following properties must be present in `build.properties`:

- `bundle.name`
- `bundle.release`
- `bundle.type`
- `bundle.format`

### Validation Command

```bash
gradle validateProperties
```

This checks that all required properties are present and non-empty.

## Customization

### Changing Build Output Directory

Edit `build.properties`:

```properties
build.path = D:/MyCustomBuildPath
```

### Changing Archive Format

Edit `build.properties`:

```properties
bundle.format = zip
```

### Changing Bundle Type

Edit `build.properties`:

```properties
bundle.type = tools
```

## Best Practices

### 1. Version Control

- Commit `build.properties` to version control
- Document any custom settings
- Use comments for optional properties

### 2. Path Configuration

- Use absolute paths for `build.path`
- Ensure build directory has write permissions
- Keep build output separate from source

### 3. Archive Format

- Use `7z` for maximum compression
- Use `zip` for better compatibility
- Consider target platform requirements

### 4. Validation

- Run `gradle validateProperties` before builds
- Run `gradle verify` to check environment
- Test configuration changes

## Troubleshooting

### Property Not Found

If you get "property not found" errors:

1. Check `build.properties` exists
2. Verify property names are correct
3. Ensure no typos in property keys
4. Run `gradle validateProperties`

### Path Issues

If you get path-related errors:

1. Check paths use forward slashes or escaped backslashes
2. Verify directories exist and are accessible
3. Check file permissions
4. Use absolute paths when possible

### Format Issues

If archive creation fails:

1. Verify `bundle.format` is `7z` or `zip`
2. Check 7-Zip is installed (for 7z format)
3. Run `gradle verify` to check environment
4. Check available disk space

## Related Documentation

- **QUICK_REFERENCE.md** - Common commands and usage
- **FEATURE_SUMMARY.md** - Build system features
- **MODULES_UNTOUCHED_INTEGRATION.md** - Remote integration details
