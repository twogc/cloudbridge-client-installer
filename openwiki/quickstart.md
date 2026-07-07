# OpenWiki Quickstart

## What this repository is
This repository builds a Go-based CloudBridge Relay client. The main binary lives in `cmd/cloudbridge-client`, with the client implementation in `pkg/relay`. The client connects to a relay server, performs JWT-based registration, and requests a tunnel for a chosen local port.

The repo also includes a shell installer (`install.sh`), a local multi-platform build script (`build.sh`), and GitHub Actions workflows for CI and tagged releases.

## Start here
- [Architecture overview](architecture.md)
- [Relay protocol and client domain model](domain/relay-protocol.md)
- [Build and release workflow](workflows/release-and-build.md)
- [Installation and packaging notes](operations/installation.md)

## Practical first steps
- Read `cmd/cloudbridge-client/main.go` to see the execution flow.
- Read `pkg/relay/config.go` for flag validation and TLS setup.
- Read `pkg/relay/client.go` for the JSON protocol and error handling.
- Check `.github/workflows/build.yml` and `.github/workflows/release.yml` before changing packaging or release behavior.

## Key behaviors
- TLS is enabled by default; the client can also be configured with client certs and a CA bundle.
- Registration uses a JWT token supplied on the command line.
- The tunnel targets a local port, defaulting to `3389`.
- The client logs the platform it is running on and handles SIGINT/SIGTERM for graceful shutdown.

## Repository map
- `cmd/cloudbridge-client/` — CLI entrypoint and runtime wiring
- `pkg/relay/` — relay client, configuration, and protocol types
- `install.sh` — root-level installer script for Linux/systemd environments
- `build.sh` — cross-platform build helper
- `.github/workflows/` — CI and release automation

## Notes for future agents
If you change connection behavior, document both the CLI flag surface and the message flow together. If you change packaging or release artifacts, update the workflow notes and the installer notes in tandem.
