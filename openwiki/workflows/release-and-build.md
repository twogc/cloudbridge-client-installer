# Build and Release Workflow

## Local build path
`build.sh` builds release-style binaries for:

- Windows amd64 and arm64
- Linux amd64 and arm64
- macOS amd64 and arm64

It writes artifacts into `build/` and creates `checksums.txt` from the generated files.

## GitHub Actions CI
`.github/workflows/build.yml` runs on push and pull request to `main` and builds one artifact per runner OS:

- Ubuntu builds `cloudbridge-client-linux-amd64`
- Windows builds `cloudbridge-client-windows-amd64.exe`
- macOS builds `cloudbridge-client-darwin-amd64`

Each job also writes a SHA-256 checksum file and uploads both files as artifacts.

## GitHub Actions release workflow
`.github/workflows/release.yml` runs on tags matching `v*`. It builds the multi-platform binaries, creates checksums, creates a GitHub Release, and uploads release assets for each platform plus the checksum file.

## Packaging/installer notes
`install.sh` is a root-level shell installer that assumes:

- root privileges
- `systemd` availability
- `curl` availability

It downloads the client binary, a systemd service file, and a default config from remote GitHub paths, then enables and starts the service.

## Change watch-outs
- The release workflow currently uses `actions/create-release` and `actions/upload-release-asset`; if you modernize release automation, update this page and the installer notes together.
- `build.sh` and the release workflow should stay in sync on platform matrix and artifact naming.
- `install.sh` references remote files under paths that are not present in this repository snapshot, so treat it as an external packaging script rather than a self-contained installer spec.
