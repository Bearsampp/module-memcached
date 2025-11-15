# Interactive Mode

## Overview

The `gradle release` task supports both interactive and non-interactive modes, allowing you to choose the most convenient method for building releases.

## Non-Interactive Mode

Specify the version directly on the command line:

```bash
gradle release -PbundleVersion=1.6.29
```

**Use when:**
- Running in CI/CD pipelines
- Automating builds with scripts
- You know exactly which version to build

## Interactive Mode

Run without specifying a version to enter interactive mode:

```bash
gradle release
```

### Interactive Mode Flow

1. **Display Available Versions**
   ```
   ======================================================================
   Interactive Release Mode
   ======================================================================

   Available versions:
     1. 1.6.15          [bin]
     2. 1.6.17          [bin]
     3. 1.6.18          [bin]
     4. 1.6.21          [bin]
     5. 1.6.24          [bin]
     6. 1.6.29          [bin]
     7. 1.6.31          [bin]
     8. 1.6.32          [bin]
     9. 1.6.33          [bin]
    10. 1.6.36          [bin]
    11. 1.6.37          [bin]
    12. 1.6.38          [bin]
    13. 1.6.39          [bin]

   Enter version to build (index or version string):
   ```

2. **Select Version**
   
   You can select a version in two ways:

   **Option A: By Index Number**
   ```
   Enter version to build (index or version string):
   5
   ```
   This will select version 1.6.24 (the 5th in the list)

   **Option B: By Version String**
   ```
   Enter version to build (index or version string):
   1.6.29
   ```
   This will select version 1.6.29 directly

3. **Confirmation**
   ```
   Selected version: 1.6.29

   ======================================================================
   Building memcached 1.6.29
   ======================================================================
   ```

4. **Build Process**
   The build proceeds normally with the selected version.

## Features

### Version Location Tags

Each version shows where it's located:
- `[bin]` - Located in `bin/` directory
- `[bin/archived]` - Located in `bin/archived/` directory

This helps you understand which versions are current and which are archived.

### Input Validation

The interactive mode validates your input:

**Invalid Index:**
```
Enter version to build (index or version string):
99

ERROR: Invalid selection index: 99
Please choose a number between 1 and 13 or enter a version string.
```

**Invalid Version String:**
```
Enter version to build (index or version string):
1.6.99

ERROR: Invalid version: 1.6.99
Please choose from available versions:
  - 1.6.15
  - 1.6.17
  ...
```

**Empty Input:**
```
Enter version to build (index or version string):
[Enter]

ERROR: No version selected. Please use non-interactive mode:
  gradle release -PbundleVersion=X.X.X

Available versions: 1.6.15, 1.6.17, 1.6.18, ...
```

### Fallback to Non-Interactive

If interactive input fails (e.g., running in a non-interactive environment), the error message provides clear instructions for using non-interactive mode:

```
ERROR: Failed to read input. Please use non-interactive mode:
  gradle release -PbundleVersion=X.X.X

Available versions: 1.6.15, 1.6.17, 1.6.18, ...
```

## Use Cases

### Interactive Mode is Best For:

1. **Manual Builds**
   - Building releases locally
   - Testing different versions
   - Exploring available versions

2. **Development**
   - Quick iteration during development
   - Testing build process
   - Verifying version availability

3. **Learning**
   - Understanding what versions are available
   - Seeing the directory structure
   - Getting familiar with the build system

### Non-Interactive Mode is Best For:

1. **Automation**
   - CI/CD pipelines
   - Build scripts
   - Scheduled builds

2. **Scripting**
   - Batch processing
   - Integration with other tools
   - Automated testing

3. **Remote Execution**
   - SSH sessions without TTY
   - Background jobs
   - Cron jobs

## PowerShell Note

When using PowerShell with non-interactive mode, quote the parameter:

```powershell
# Correct
gradle release "-PbundleVersion=1.6.29"

# Incorrect (PowerShell interprets -P as a parameter)
gradle release -PbundleVersion=1.6.29
```

In CMD, quotes are optional:

```cmd
gradle release -PbundleVersion=1.6.29
```

## Examples

### Example 1: Interactive Selection by Index

```bash
$ gradle release

======================================================================
Interactive Release Mode
======================================================================

Available versions:
  1. 1.6.29          [bin]
  2. 1.6.31          [bin]
  3. 1.6.32          [bin]

Enter version to build (index or version string):
2

Selected version: 1.6.31

======================================================================
Building memcached 1.6.31
======================================================================
...
```

### Example 2: Interactive Selection by Version String

```bash
$ gradle release

======================================================================
Interactive Release Mode
======================================================================

Available versions:
  1. 1.6.29          [bin]
  2. 1.6.31          [bin]
  3. 1.6.32          [bin]

Enter version to build (index or version string):
1.6.32

Selected version: 1.6.32

======================================================================
Building memcached 1.6.32
======================================================================
...
```

### Example 3: Non-Interactive Mode

```bash
$ gradle release -PbundleVersion=1.6.29

======================================================================
Building memcached 1.6.29
======================================================================
...
```

## Comparison with Bruno Module

The interactive mode implementation matches the bruno module's behavior:

✅ Same interactive prompt format
✅ Same version selection methods (index or string)
✅ Same validation logic
✅ Same error messages
✅ Same fallback behavior
✅ Same location tags display

This ensures consistency across all Bearsampp modules.

## Related Documentation

- **QUICK_REFERENCE.md** - Quick command reference
- **FEATURE_SUMMARY.md** - Overview of all features
- **README.md** - Main documentation
