# VSCodium Patches Classification

This document provides a comprehensive classification and summary of all patches applied to the VSCode source code to create VSCodium.

## Patch Application Process

Patches are applied by [prepare_vscode.sh](prepare_vscode.sh) in the following order:

1. **Main patches** (`patches/*.patch`) - Applied to all builds
2. **Insider patches** (`patches/insider/*.patch`) - Applied only when `VSCODE_QUALITY=insider`
3. **OS-specific patches** (`patches/${OS_NAME}/*.patch`) - Applied based on the operating system
4. **User patches** (`patches/user/*.patch`) - Custom user-defined patches
5. **Special patches** (`patches/*.patch.yet`) - Conditionally applied (e.g., `disable-update.patch.yet` when `DISABLE_UPDATE=yes`)

## Patch Categories

### 1. Branding & Identity

These patches rebrand VSCode to VSCodium, changing names, identifiers, and references throughout the codebase.

- **[brand.patch](patches/brand.patch)** - Main branding changes throughout the codebase, replacing Microsoft/VSCode branding with VSCodium
- **[binary-name.patch](patches/binary-name.patch)** - Changes binary names from `code` to use `product.applicationName` (codium/codium-insiders)

### 2. Privacy & Telemetry

Patches that disable or modify telemetry and tracking features to protect user privacy.

- **[telemetry.patch](patches/telemetry.patch)** - Disables telemetry by default:

  - Changes `telemetry.telemetryLevel` default from `ON` to `OFF`
  - Disables `telemetry.enableTelemetry` by default
  - Disables crash reporting by default
  - Disables natural language search (uses Microsoft online service)
  - Disables edit telemetry collection
  - Disables experiment fetching from Microsoft services

- **[disable-settingssync-logger.patch](patches/disable-settingssync-logger.patch)** - Disables settings sync logging

### 3. Microsoft Feature Disabling

Patches that disable Microsoft-specific features and integrations.

- **[disable-copilot.patch](patches/disable-copilot.patch)** - Disables GitHub Copilot and AI features:

  - Sets `chat.disableAIFeatures` default to `true`
  - Removes Copilot UI elements and menu items
  - Disables chat setup triggers
  - Hides AI-related views and commands

- **[disable-cloud.patch](patches/disable-cloud.patch)** - Disables Microsoft cloud features:

  - Removes cloud session/edit sync sign-in actions
  - Disables cloud changes functionality

- **[disable-vscodedev.patch](patches/disable-vscodedev.patch)** - Removes vscode.dev integration:

  - Removes "Open in vscode.dev" commands
  - Removes vscode.dev link copying
  - Removes "Continue Working in vscode.dev" option

- **[disable-signature-verification.patch](patches/disable-signature-verification.patch)** - Disables extension signature verification (Microsoft-signed extensions)

### 4. VSCodium Feature Additions

Patches that add new features specific to VSCodium.

- **[feat-announcements.patch](patches/feat-announcements.patch)** - Adds VSCodium announcements system:

  - Adds announcement list to welcome page
  - Fetches announcements from VSCodium GitHub repository
  - Adds `workbench.welcomePage.extraAnnouncements` setting
  - Supports both built-in and remote announcements

- **[feat-command-filter.patch](patches/feat-command-filter.patch)** - Adds command authorization filtering:

  - Adds `commands.filters` configuration
  - Allows blocking/authorizing specific commands
  - Supports 'ask', 'off', 'on' modes for command execution

- **[feat-ext-unsafe.patch](patches/feat-ext-unsafe.patch)** - Adds support for marking extensions as unsafe

- **[feat-user-product.patch](patches/feat-user-product.patch)** - Allows user-customizable product.json:
  - Loads custom product.json from user data directory
  - Merges user customizations with default product configuration
  - Enables users to override product settings without rebuilding

### 5. Extension Management

Patches related to extension gallery and extension handling.

- **[fix-gallery.patch](patches/fix-gallery.patch)** - Fixes extension gallery configuration:

  - Adds `itemUrl` and `latestUrlTemplate` to extension gallery config
  - Supports environment variable overrides for gallery URLs
  - Enables custom extension marketplace configuration

- **[ext-from-gh.patch](patches/ext-from-gh.patch)** - Enables installing extensions from GitHub

- **[extensions-disable-mangler.patch](patches/extensions-disable-mangler.patch)** - Disables code mangling for extensions

### 6. Build & Packaging

Patches that modify the build and packaging process.

- **[version-0-release.patch](patches/version-0-release.patch)** - Removes timestamp-based package revision:

  - Uses clean version numbers instead of adding timestamps
  - Affects Debian, RPM, and Snap packages
  - Normalizes version format for extension API

- **[version-1-update.patch](patches/version-1-update.patch)** - Additional version handling updates

- **[sourcemaps.patch](patches/sourcemaps.patch)** - Enables/modifies source map generation

- **[remove-mangle.patch](patches/remove-mangle.patch)** - Removes code mangling/minification

- **[optional-tree-sitter.patch](patches/optional-tree-sitter.patch)** - Makes tree-sitter parsing optional

### 7. CLI & Command Line

Patches affecting the command-line interface.

- **[cli.patch](patches/cli.patch)** - Updates CLI for VSCodium:
  - Changes environment variable names (`VSCODE_CLI_UPDATE_ENDPOINT` → `VSCODE_CLI_UPDATE_ENDPOINT`)
  - Adds `VSCODE_CLI_DOWNLOAD_ENDPOINT`, `VSCODE_CLI_APP_NAME`, `VSCODE_CLI_BINARY_NAME` support
  - Updates server entrypoint handling
  - Modifies download and update mechanisms

### 8. Remote Development

Patches for remote server and development features.

- **[add-remote-url.patch](patches/add-remote-url.patch)** - Adds `serverDownloadUrlTemplate` to product.json:

  - Points to VSCodium releases on GitHub
  - Enables remote server downloads from VSCodium repository

- **[fix-remote-libs.patch](patches/fix-remote-libs.patch)** - Fixes remote development library handling

### 9. UI & UX Fixes

Patches that fix UI/UX issues or customize user interface.

- **[fix-eol-banner.patch](patches/fix-eol-banner.patch)** - Fixes end-of-life banner display

- **[report-issue.patch](patches/report-issue.patch)** - Updates issue reporting:

  - Changes GitHub repository reference to VSCodium
  - Removes Microsoft's duplicate detection service
  - Uses direct GitHub issue search instead

- **[terminal-suggest.patch](patches/terminal-suggest.patch)** - Terminal suggestion modifications

### 10. Windows Group Policies

Patches for Windows enterprise policy support.

- **[policies.patch](patches/policies.patch)** - Updates Windows policy registry paths:
  - Changes from `Software\Policies\Microsoft\` to `Software\Policies\VSCodium\`
  - Updates ADMX namespace from `Microsoft.Policies.*` to organization name
  - Changes policy watcher package from `@vscode/policy-watcher` to `@vscodium/policy-watcher`

### 11. Authentication & GitHub Integration

Patches modifying authentication and GitHub-related features.

- **[use-github-pat.patch](patches/use-github-pat.patch)** - Disables built-in GitHub authentication:
  - Returns `false` for `isSupportedClient()` check
  - Prevents automatic GitHub OAuth flow
  - Disables hosted GitHub Enterprise detection

### 12. Update Management

Patches controlling application updates.

- **[update-cache-path.patch](patches/update-cache-path.patch)** - Updates cache path to use application name:

  - Changes from `vscode-{quality}-{target}-{arch}` to `{applicationName}-{quality}-{target}-{arch}`
  - Prevents conflicts between VS Code and VSCodium updates

- **[disable-update.patch.yet](patches/disable-update.patch.yet)** - Conditionally applied to disable updates completely when `DISABLE_UPDATE=yes`

---

## Platform-Specific Patches

### Linux (`patches/linux/`)

#### Architecture Support

- **[arch-0-support.patch](patches/linux/arch-0-support.patch)** - Base architecture support
- **[arch-1-ppc64le.patch](patches/linux/arch-1-ppc64le.patch)** - PowerPC 64-bit little-endian support
- **[arch-2-riscv64.patch](patches/linux/arch-2-riscv64.patch)** - RISC-V 64-bit support
- **[arch-3-loong64.patch](patches/linux/arch-3-loong64.patch)** - LoongArch 64-bit support
- **[arch-4-s390x.patch](patches/linux/arch-4-s390x.patch)** - IBM s390x (mainframe) support

#### Build & Package Fixes

- **[fix-build.patch](patches/linux/fix-build.patch)** - Linux build fixes
- **[fix-dependencies.patch](patches/linux/fix-dependencies.patch)** - Linux dependency fixes
- **[fix-npm-postinstall.patch](patches/linux/fix-npm-postinstall.patch)** - NPM post-install fixes for Linux
- **[fix-reh-bootstrap.patch](patches/linux/fix-reh-bootstrap.patch)** - Remote extension host bootstrap fixes
- **[rpm.patch](patches/linux/rpm.patch)** - RPM package specific fixes
- **[update-xdg-path.patch](patches/linux/update-xdg-path.patch)** - XDG path updates for Linux

#### Linux Client Patches

- **[client/disable-remote.patch](patches/linux/client/disable-remote.patch)** - Disables remote features in client
- **[client/avoid-crash-16k-page-size.patch](patches/linux/client/avoid-crash-16k-page-size.patch)** - Fixes crashes on systems with 16KB page size

#### Linux Remote Extension Host

- **[reh/s390x/arch-4-s390x-package.json.patch](patches/linux/reh/s390x/arch-4-s390x-package.json.patch)** - s390x package.json for remote extension host

### macOS (`patches/osx/`)

- **[fix-emulated-urls.patch](patches/osx/fix-emulated-urls.patch)** - Fixes URL handling in emulated environments on macOS

### Windows (`patches/windows/`)

- **[appx.patch](patches/windows/appx.patch)** - Windows APPX package modifications
- **[cli.patch](patches/windows/cli.patch)** - Windows-specific CLI modifications
- **[win7.patch](patches/windows/win7.patch)** - Windows 7 compatibility fixes

### Alpine (`patches/alpine/`)

- **[reh/fix-node-docker.patch](patches/alpine/reh/fix-node-docker.patch)** - Fixes Node.js issues in Alpine/Docker for remote extension host

### Helper Patches (`patches/helper/`)

- **[settings.patch](patches/helper/settings.patch)** - Helper settings modifications

### Insider-Specific (`patches/insider/`)

- **[system-extensions.patch](patches/insider/system-extensions.patch)** - System extension handling for Insider builds

---

## Summary Statistics

- **Total patches**: 51
- **Main patches**: 28
- **Linux-specific patches**: 16
- **Windows-specific patches**: 3
- **macOS-specific patches**: 1
- **Alpine-specific patches**: 1
- **Insider-specific patches**: 1
- **Helper patches**: 1
- **Conditional patches**: 1 (.yet file)

## Key Transformations

The patches collectively transform VS Code into VSCodium by:

1. **Removing Microsoft branding** and replacing with VSCodium identity
2. **Disabling telemetry and tracking** by default for privacy
3. **Removing proprietary features** (Copilot, vscode.dev, cloud sync)
4. **Changing extension marketplace** to Open VSX by default
5. **Adding user customization** features (user product.json, command filters)
6. **Supporting additional architectures** (ppc64le, riscv64, loong64, s390x)
7. **Enabling self-hosting** with custom update URLs and repositories
8. **Improving transparency** by removing obfuscation/mangling
9. **Adding VSCodium-specific features** (announcements, customization options)
10. **Ensuring independence** from Microsoft services and infrastructure

## Maintenance Notes

When updating to a new VS Code version:

1. Review all patches for conflicts with upstream changes
2. Test platform-specific patches on respective platforms
3. Verify branding changes are comprehensive
4. Ensure privacy settings remain disabled by default
5. Check that extension gallery points to Open VSX
6. Test update mechanisms with VSCodium repositories
7. Validate architecture-specific patches on respective platforms
