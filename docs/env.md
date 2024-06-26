# Configuring

txtdot can be configured either with environment variables
or with the `.env` file in the working directory which has higher priority.
For sample config, see [`.env.example`](https://github.com/TxtDot/txtdot/blob/main/.env.example).

## Server Settings

### HOST

Default: `0.0.0.0`

Host where HTTP server should listen for connections.
Set it to `127.0.0.1` if your txtdot instance is behind reverse proxy,
`0.0.0.0` otherwise.

### PORT

Default: `8080`

Port where HTTP server should listen for connections.

### Timeout

Default: `0`

Max response time in milliseconds. If it's reached, the request is aborted. If set to `0`, the timeout is disabled.

### REVERSE_PROXY

Default: `false`

Set it to `true` only if your txtdot instance runs behind reverse proxy.
Needed for processing X-Forwarded headers.

## Proxy

### PROXY_RES

Default: `true`

Whether to allow proxying images, video, audio
and everything else through your txtdot instance.

### IMG_COMPRESS

Default: `true`

Whether to compress images through your txtdot instance.

## Documentation

### SWAGGER

Default: `false`

Whether to add `/doc` route for Swagger API docs.

## Third-party

### SEARX_URL

SearXNG base URL, if set, txtdot will use it for searching and add search form to the page with /search route.

### WEBDER_URL

Webder base URL, if set, txtdot will use it for rendering web pages.
