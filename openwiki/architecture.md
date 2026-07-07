# Architecture Overview

## System shape
This repository contains a single executable client that wraps the `pkg/relay` package.

- `cmd/cloudbridge-client/main.go` parses flags, validates configuration, creates TLS configuration when required, connects to the relay, registers with a JWT, and starts the tunnel.
- `pkg/relay/config.go` owns configuration fields, validation, and TLS config construction.
- `pkg/relay/client.go` owns the network connection, JSON encoding/decoding, tunnel creation, and relay-specific error translation.

## Runtime flow
1. Parse command-line flags into `relay.Config`.
2. Validate host, port, local port, and TLS certificate/key pairing.
3. Build a `tls.Config` when TLS is enabled.
4. Connect to the relay server over TCP or TLS.
5. Send a `register` message containing the JWT token.
6. Send a `connect` message containing the requested local port.
7. Wait for termination signals and close the connection on exit.

## Source-level details
### Entrypoint
`cmd/cloudbridge-client/main.go` shows the exact runtime sequence and the default values used by the CLI.

### Configuration and TLS
`pkg/relay/config.go` establishes the minimum TLS version (`TLS 1.2`), optionally loads a client certificate/key pair, and optionally loads a CA bundle into `RootCAs`.

### Client behavior
`pkg/relay/client.go` uses `encoding/json` over a `net.Conn`. The client currently expects a simple `{status, error}` response shape and treats any non-`ok` status as failure.

## Change watch-outs
- The code currently references reconnection settings in `Config`, but the visible client flow in `main.go` does not implement a reconnect loop. Treat those flags as configuration surface, not as proof of active reconnect behavior.
- `main.go` imports `tls.Config` but the file excerpt in this repo snapshot does not show the `crypto/tls` import; if you edit the entrypoint, make sure the file still compiles.
- `HandleError` in `pkg/relay/client.go` switches on error-string constants, so changing server error strings will affect client-side behavior.

## Best starting points for future changes
- Connection or auth flow changes: `cmd/cloudbridge-client/main.go` and `pkg/relay/client.go`
- TLS and certificate handling: `pkg/relay/config.go`
- CLI defaults and validation: `cmd/cloudbridge-client/main.go` and `pkg/relay/config.go`
