# Configuring

txtdot can be configured either with environment variables
or with the `.env` file in the working directory which has higher priority.
For sample config, see [`.env.example`](https://github.com/TxtDot/txtdot/blob/main/.env.example).

## HOST

Default: `0.0.0.0`

Host where HTTP server should listen for connections.
Set it to `127.0.0.1` if your txtdot instance is behind reverse proxy,
`0.0.0.0` otherwise.

## PORT

Default: `8080`

Port where HTTP server should listen for connections.

## REVERSE_PROXY

Default: `false`

Set it to `true` only if your txtdot instance runs behind reverse proxy.
Needed for processing X-Forwarded headers.
