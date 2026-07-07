# Installation and Packaging

## Installation paths in the repository
There are two relevant installation stories in this repo:

1. The README documents `go install` and downloadable prebuilt binaries.
2. `install.sh` automates a Linux/systemd installation from remote release assets.

## What `install.sh` does
The installer script:

- requires root
- checks for `systemd` and `curl`
- creates config, data, and log directories
- creates a `cloudbridge-client` system user if needed
- downloads the client binary from GitHub releases
- downloads a systemd service file and a default config
- enables and starts the service

## Operational assumptions
The installer is clearly aimed at Linux environments that use `systemd`. It is not a universal install path for every platform supported by the Go binary.

## Change watch-outs
- If you change release artifact names, update `install.sh` and the release workflow together.
- If you change the binary name or default config location, check the installer script for hard-coded paths.
- Because the script downloads remote resources, it is sensitive to repository name changes and release layout changes.

## Where to look next
- `README.md` for user-facing install and usage examples
- `build.sh` for local cross-platform builds
- `.github/workflows/release.yml` for the source of release artifacts
