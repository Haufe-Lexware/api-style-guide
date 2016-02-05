## API Management

This is work in process.

### The `X-Api-Host` header

Whenever the API Gateway makes a call to a backend API service, it MUST add a `X-Api-Host` custom header to the call. This header is needed to correctly assembly [hypermedia](response-format.md) links wherever absolute URIs are needed.

See also [Hypermedia and REST](hypermedia-and-rest.md) for a discussion of this custom header.

#### Implementation in Azure API Management

By applying a custom policy on the Product level, you can insert this header automatically:

```xml
<inbound>
	<set-header name="X-Api-Host" exists-action="override">
		<value>@(context.Request.OriginalUrl.Scheme + "://" + context.Request.OriginalUrl.Host + context.Api.Path)</value>
	</set-header>
	<base />
</inbound>
```

Example: Your end point is called at `https://api.contenthub.haufe.de/search/v1/query?q=Steuerhinterziehung`. The API prefix is `/search/v1`, and the operation is `/query`. In this case, the header will look as follows:

```
X-Api-Host: https://api.contenthub.haufe.de/search/v1
```
