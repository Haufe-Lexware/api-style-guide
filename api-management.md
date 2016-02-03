## API Management

This is work in process.

### The `X-Api-Host` header

Whenever the API Gateway makes a call to a backend API service, it MUST add a `X-Api-Host` custom header to the call. This header is needed to correctly assembly [hypermedia](response-format.md) links wherever absolute URIs are needed.

See also [Hypermedia and REST](hypermedia-and-rest.md) for a discussion of this custom header.