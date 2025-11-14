# Release Process Guide

Complete guide for creating and publishing releases of Bearsampp Module Memcached.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Release Workflow](#release-workflow)
- [Step-by-Step Guide](#step-by-step-guide)
- [Version Management](#version-management)
- [Publishing Releases](#publishing-releases)
- [Post-Release Tasks](#post-release-tasks)
- [Troubleshooting](#troubleshooting)

## Overview

The release process involves:

1. **Preparation**     - Verify environment and version
2. **Building**        - Create release package
3. **Testing**         - Verify package integrity
4. **Publishing**      - Upload to GitHub releases
5. **Documentation**   - Update release notes and properties

### Release Timeline

```
┌─────────────┐
│ Preparation │ (15 min)
└──────┬──────┘
       │
       ▼
┌─��───────────┐
│  Building   │ (5 min)
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Testing   │ (10 min)
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Publishing  │ (10 min)
└──────┬──────┘
       │
       ▼
┌─────────────┐
│    Done     │
└─────────────┘

Total: ~40 minutes
```

---

## Prerequisites

### Required Tools

| Tool              | Version      | Purpose                                    | Installation                           |
|------------------|--------------|--------------------------------------------|-----------------------------------------|
| Java             | 8+           | Run Gradle build                           | [Download](https://adoptium.net/)       |
| Gradle           | 7.0+         | Build automation                           | [Download](https://gradle.org/)         |
| 7-Zip            | Latest       | Create .7z archives                        | [Download](https://www.7-zip.org/)      |
| Git              | Latest       | Version control                            | [Download](https://git-scm.com/)        |

### Required Access

| Access Type          | Purpose                                    | Required For                           |
|---------------------|--------------------------------------------|-----------------------------------------|
| GitHub Write        | Push commits and tags                      | Publishing releases                     |
| GitHub Releases     | Create and upload releases                 | Publishing releases                     |

### Environment Setup

```bash
# Verify Java
java -version

# Verify Gradle
gradle --version

# Verify 7-Zip
7z

# Verify Git
git --version

# Verify build environment
gradle verify
```

**Expected Output:**

```
╔════════════════════════════════════════════════════════════════════════════╗
║                    Build Environment Verification                          ║
╚════════════════════════════════════════════════════════════════════════════╝

Environment Check Results:
──────────────────────────────────���──────────────────────────────────��──────
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

---

## Release Workflow

### Standard Release Flow

```
1. Check for new Memcached version
   ↓
2. Download and prepare binaries
   ↓
3. Add to bin/ directory
   ↓
4. Update build.properties
   ↓
5. Build release package
   ↓
6. Test package
   ↓
7. Create Git tag
   ↓
8. Push to GitHub
   ↓
9. Create GitHub release
   ↓
10. Upload package
   ↓
11. Update releases.properties
   ↓
12. Commit and push
```

---

## Step-by-Step Guide

### Step 1: Check for New Version

Check Memcached official releases:

**Source:** https://memcached.org/downloads

**Example:**
```
Latest version: 1.6.40
Current version: 1.6.39
Action: Proceed with release
```

---

### Step 2: Download Binaries

Download Windows binaries for the new version.

**Sources:**
- Official Memcached builds
- Third-party Windows builds
- Compiled from source

**Verify:**
- `memcached.exe` is present
- Version matches expected
- All dependencies included

---

### Step 3: Prepare Binary Directory

Create version directory in `bin/`:

```bash
# Create directory
mkdir bin/memcached1.6.40

# Copy binaries
copy memcached.exe bin/memcached1.6.40/
copy *.dll bin/memcached1.6.40/

# Verify
dir bin/memcached1.6.40
```

**Expected Structure:**
```
bin/memcached1.6.40/
├── memcached.exe
├── pthreadGC2.dll
└── README.txt (optional)
```

---

### Step 4: Update Configuration

Update `build.properties` with new release date:

```properties
bundle.name = memcached
bundle.release = 2025.9.15
bundle.type = bins
bundle.format = 7z
```

**Release Date Format:** `YYYY.M.D` or `YYYY.MM.DD`

**Examples:**
- `2025.9.15` - September 15, 2025
- `2025.12.1` - December 1, 2025

---

### Step 5: Verify Configuration

```bash
# Validate properties
gradle validateProperties

# List available versions
gradle listVersions

# Verify environment
gradle verify
```

**Expected Output:**

```
╔════════════════════════════════════════════════════════════════════════════╗
║                    Validating build.properties                             ║
╚════════════════════════════════════════════════════════════════════════════╝

[SUCCESS] All required properties are present:

Property             Value
──────────────────────────────────────────────────────
bundle.name          memcached
bundle.release       2025.9.15
bundle.type          bins
bundle.format        7z
──────────────────────────────────────────────────────
```

---

### Step 6: Build Release Package

Build the release package:

```bash
gradle release -PbundleVersion=1.6.40
```

**Expected Output:**

```
╔════════════════════════════════════════════════════════════════════════════╗
║                         Release Build                                      ║
╚════════════════════════════════════════════════════════════════════════════╝

Building release for memcached version 1.6.40...

Bundle path: E:/Bearsampp-development/module-memcached/bin/memcached1.6.40

Preparing bundle...
Copying bundle files...
Creating archive: bearsampp-memcached-1.6.40-2025.9.15.7z

╔════════════════════════════════════════════════════════════════════════════╗
║                      Release Build Completed                               ║
╚════════════════════════════════════════════════════════════════════════════╝

Release package created:
  C:\Users\troy\Bearsampp-build\release\bearsampp-memcached-1.6.40-2025.9.15.7z

Package size: 0.45 MB
```

**Output Location:**
```
${buildPath}/release/bearsampp-memcached-1.6.40-2025.9.15.7z
```

---

### Step 7: Test Package

Verify the release package:

```bash
# Extract to test directory
7z x bearsampp-memcached-1.6.40-2025.9.15.7z -otest/

# Verify contents
dir test/memcached1.6.40

# Test executable
test/memcached1.6.40/memcached.exe -h
```

**Verification Checklist:**

- [ ] Archive extracts without errors
- [ ] All files present
- [ ] `memcached.exe` runs
- [ ] Version is correct
- [ ] No missing dependencies

---

### Step 8: Create Git Tag

Create and push Git tag:

```bash
# Stage changes
git add bin/memcached1.6.40/
git add build.properties

# Commit
git commit -m "Add memcached 1.6.40"

# Create tag
git tag -a 2025.9.15 -m "Release 2025.9.15 - Memcached 1.6.40"

# Push commit
git push origin main

# Push tag
git push origin 2025.9.15
```

**Tag Format:** `YYYY.M.D` or `YYYY.MM.DD`

**Tag Message Format:**
```
Release YYYY.M.D - Memcached X.X.X

- Add memcached X.X.X
- Update build configuration
- Update documentation
```

---

### Step 9: Create GitHub Release

Create release on GitHub:

**URL:** https://github.com/bearsampp/module-memcached/releases/new

**Release Form:**

| Field                | Value                                                          |
|---------------------|----------------------------------------------------------------|
| Tag                 | `2025.9.15`                                                    |
| Release Title       | `Memcached 1.6.40 - 2025.9.15`                                 |
| Description         | See template below                                             |
| Attach Files        | `bearsampp-memcached-1.6.40-2025.9.15.7z`                      |

**Release Description Template:**

```markdown
# Memcached 1.6.40 - Release 2025.9.15

## What's New

- Updated to Memcached 1.6.40
- Latest Windows binaries
- Compatible with Bearsampp 2025.x

## Download

- **File:** bearsampp-memcached-1.6.40-2025.9.15.7z
- **Size:** 0.45 MB
- **Format:** 7z

## Installation

1. Download the archive
2. Extract to Bearsampp modules directory
3. Restart Bearsampp

## Changelog

### Memcached 1.6.40

- Bug fixes and improvements
- Performance enhancements
- Security updates

## Requirements

- Bearsampp 2025.x or later
- Windows 10/11 (64-bit)

## Links

- [Memcached Official](https://memcached.org/)
- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [Documentation](https://bearsampp.com/module/memcached)

## Support

Report issues at: https://github.com/bearsampp/bearsampp/issues
```

---

### Step 10: Upload Package

Upload the release package to GitHub:

1. Go to the release page
2. Click "Edit release"
3. Drag and drop `bearsampp-memcached-1.6.40-2025.9.15.7z`
4. Wait for upload to complete
5. Click "Update release"

**Verify Upload:**
- File appears in release assets
- Download link works
- File size is correct

---

### Step 11: Update releases.properties

Add new release entry:

```properties
# releases.properties

# ... existing entries ...

1.6.40 = https://github.com/Bearsampp/module-memcached/releases/download/2025.9.15/bearsampp-memcached-1.6.40-2025.9.15.7z
```

**URL Format:**
```
https://github.com/Bearsampp/module-memcached/releases/download/{tag}/bearsampp-memcached-{version}-{release}.7z
```

**Sort Order:** Ascending by version number

---

### Step 12: Commit and Push

Commit the updated releases.properties:

```bash
# Stage changes
git add releases.properties

# Commit
git commit -m "Update releases.properties for 1.6.40"

# Push
git push origin main
```

---

### Step 13: Verify Release

Verify the release is complete:

```bash
# List releases
gradle listReleases

# Check latest release
curl -s https://api.github.com/repos/bearsampp/module-memcached/releases/latest | grep tag_name
```

**Verification Checklist:**

- [ ] Release appears on GitHub
- [ ] Package is downloadable
- [ ] releases.properties is updated
- [ ] Git tag exists
- [ ] Documentation is updated

---

## Version Management

### Version Numbering

Memcached uses semantic versioning:

```
MAJOR.MINOR.PATCH
  │     │     │
  │     │     └─── Bug fixes
  │     └───────── New features (backward compatible)
  └─────────────── Breaking changes
```

**Examples:**
- `1.6.39` - Version 1, minor 6, patch 39
- `1.6.40` - Version 1, minor 6, patch 40
- `2.0.0` - Version 2, minor 0, patch 0

### Release Numbering

Bearsampp releases use date-based versioning:

```
YYYY.M.D
  │   │ │
  │   │ └─── Day
  │   └───── Month
  └───────── Year
```

**Examples:**
- `2025.9.15` - September 15, 2025
- `2025.12.1` - December 1, 2025
- `2026.1.20` - January 20, 2026

### Version Matrix

| Memcached Version | Release Date | Bearsampp Release | Status      |
|------------------|--------------|-------------------|-------------|
| 1.6.39           | 2025.8.20    | 2025.8.20         | Current     |
| 1.6.38           | 2025.4.19    | 2025.4.19         | Supported   |
| 1.6.36           | 2025.2.11    | 2025.2.11         | Supported   |
| 1.6.33           | 2024.12.23   | 2024.12.23        | Supported   |
| 1.6.32           | 2024.12.1    | 2024.12.1         | Supported   |

---

## Publishing Releases

### GitHub Release Checklist

- [ ] Tag created and pushed
- [ ] Release created on GitHub
- [ ] Release title follows format
- [ ] Release description is complete
- [ ] Package uploaded
- [ ] Package is downloadable
- [ ] releases.properties updated
- [ ] Documentation updated

### Release Assets

Each release should include:

| Asset                                          | Type     | Required | Description                    |
|-----------------------------------------------|----------|----------|--------------------------------|
| `bearsampp-memcached-{version}-{date}.7z`     | Binary   | Yes      | Release package                |
| Release notes                                 | Text     | Yes      | Changelog and information      |

### Release Metadata

GitHub release metadata:

```json
{
  "tag_name": "2025.9.15",
  "name": "Memcached 1.6.40 - 2025.9.15",
  "draft": false,
  "prerelease": false,
  "body": "Release description...",
  "assets": [
    {
      "name": "bearsampp-memcached-1.6.40-2025.9.15.7z",
      "content_type": "application/x-7z-compressed"
    }
  ]
}
```

---

## Post-Release Tasks

### Update Documentation

Update project documentation:

1. **README.md** - Update version badges
2. **.gradle-docs/** - Update documentation
3. **CHANGELOG.md** - Add release notes (if exists)

### Announce Release

Announce the release:

1. **GitHub Discussions** - Post announcement
2. **Project Website** - Update downloads page
3. **Social Media** - Share release (if applicable)

### Monitor Issues

Monitor for issues:

1. Check GitHub issues
2. Monitor download statistics
3. Review user feedback

---

## Troubleshooting

### Common Issues

#### Issue: Build fails

**Error:**
```
Bundle version not found: E:/Bearsampp-development/module-memcached/bin/memcached1.6.40
```

**Solution:**
1. Verify directory exists: `dir bin/memcached1.6.40`
2. Check directory name matches version
3. Ensure binaries are present

---

#### Issue: 7z compression fails

**Error:**
```
7z compression failed with exit code: 2
```

**Solution:**
1. Verify 7z is installed: `7z`
2. Check PATH includes 7-Zip
3. Verify disk space available
4. Check file permissions

---

#### Issue: Git push fails

**Error:**
```
! [rejected]        main -> main (fetch first)
```

**Solution:**
```bash
# Pull latest changes
git pull origin main

# Resolve conflicts if any
git mergetool

# Push again
git push origin main
```

---

#### Issue: GitHub release upload fails

**Error:**
```
Failed to upload asset
```

**Solution:**
1. Check file size (< 2GB)
2. Verify internet connection
3. Try uploading via web interface
4. Check GitHub status

---

#### Issue: Package extraction fails

**Error:**
```
Cannot open file as archive
```

**Solution:**
1. Verify package integrity
2. Re-download package
3. Rebuild package
4. Check 7z version

---

### Debug Commands

```bash
# Verify build
gradle verify --info

# Test build
gradle release -PbundleVersion=1.6.40 --debug

# Check Git status
git status
git log --oneline -5

# Verify package
7z t bearsampp-memcached-1.6.40-2025.9.15.7z

# Test extraction
7z x bearsampp-memcached-1.6.40-2025.9.15.7z -otest/
```

---

## Release Checklist

### Pre-Release

- [ ] New version available
- [ ] Binaries downloaded
- [ ] Directory created in `bin/`
- [ ] `build.properties` updated
- [ ] Environment verified
- [ ] Configuration validated

### Build

- [ ] Release package built
- [ ] Package tested
- [ ] Package size verified
- [ ] Contents verified

### Publish

- [ ] Git changes committed
- [ ] Git tag created
- [ ] Changes pushed to GitHub
- [ ] GitHub release created
- [ ] Package uploaded
- [ ] Release published

### Post-Release

- [ ] `releases.properties` updated
- [ ] Changes committed and pushed
- [ ] Release verified
- [ ] Documentation updated
- [ ] Announcement posted

---

## Best Practices

### Version Control

- Always create tags for releases
- Use descriptive commit messages
- Keep release history in `releases.properties`
- Document breaking changes

### Testing

- Test package before publishing
- Verify all files are included
- Check executable runs correctly
- Test on clean system if possible

### Documentation

- Keep documentation up-to-date
- Document known issues
- Provide clear installation instructions
- Include changelog

### Communication

- Announce releases clearly
- Respond to user feedback
- Monitor for issues
- Provide support

---

**Last Updated:** 2025-08-20  
**Version:** 2025.8.20  
**Maintainer:** Bearsampp Team
