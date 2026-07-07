# Relay Protocol and Domain Model

## What the client speaks
The client talks to a relay service using JSON messages over a network connection.

## Messages
Defined in `pkg/relay/client.go`:

- `register` message
  - `type`: `register`
  - `token`: JWT token string supplied by the user
- `connect` message
  - `type`: `connect`
  - `local_port`: local port number to tunnel

## Responses
The client expects a response with:

- `status`: expected to be `ok`
- `error`: optional error string when status is not `ok`

Any non-`ok` status is treated as failure for both registration and tunnel creation.

## Error vocabulary
The client has explicit handlers for these relay error strings:

- `invalid_token`
- `rate_limit_exceeded`
- `connection_limit_reached`
- `server_unavailable`

`HandleError` maps those strings to user-oriented messages, and in one case sleeps briefly before returning a retry-oriented message.

## Business meaning
This repository does not contain the server side, but the client code implies the business flow:

1. A user authenticates with a JWT token.
2. The relay authorizes registration.
3. The client requests a tunnel to a chosen local port.
4. The relay either accepts the connection or returns a domain-specific error.

## Change watch-outs
- Keep the JSON field names stable unless the relay server is updated in lockstep.
- If you change error strings, update both the protocol docs and any matching server implementation.
- If you add new protocol messages, define them near the existing message structs so the domain model remains easy to find.
