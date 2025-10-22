# Learn VS Codium Build Scripts

## How to build

First, set or check VS Code version in `upstream/stable.json`.

```bash
# clone vscode and build, don't package
bash ./dev/build.sh

# only clone vscode
bash ./dev/build.sh -o
# Generate `vscode/` `dev/build.env`

# only pactch and build
bash ./dev/build.sh -s
# Generate `VSCode-win32-x64` `vscode-reh-win32-x64` `vscode-reh-web-win32-x64`

# only packaging, skip msi packaging
SHOULD_BUILD_MSI=no SHOULD_BUILD_MSI_NOUP=no bash ./dev/build.sh -s -o -p
# Generate `assets`
# system setup exe
# user setup exe
# vscode zip
# vscode-cli.tar.gz
# reh.tar.gz
# reh-web.tar.gz

```

- `-o` skip build
- `-p` generate packages
- `-s` skip git clone

## Abbreviations

- REH: Remote Extension Host

## Anatomy

### Entry: `dev/build.sh`

- Parse options to skip steps
- Set environment variables
- Main phases:
  - Git clone, checkout, save commit info: `get_repo.sh`
  - Generate BUILD_SOURCEVERSION for vscodium version update: `version.sh`
  - Git rest, apply patches, configure native addons and build: `build.sh`
  - Packaging: `prepare_assets.sh`

### Build: `build.sh`

- Check and set BUILD_SOURCEVERSION: `version.sh`
- Apply patches and install dependencies: `prepare_vscode.sh`
- Run npm build tasks: `npm run <check>; npm run gulp <gulp_task>`
- Build cli, aka vscode-tunnel: `build_cli.sh`
- Build Remote Extension Host: `npm run gulp <gulp_task>`

### Packaging: `prepare_assets.sh`

- On Mac, notarize and sign, then build zip and dmg.
- On Windows, build inno-updater, then build zip, exe and msi.
- Package Remote Extension Host.
- Package vscode-cli, aka vscode-tunnel.
