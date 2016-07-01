## API Management

### API Management Scenarios

There are many different scenarions in which API management can and must be used. The following sections describe the four most basic scenarios.

#### Internal API
tbd.

#### Partner API
tbd.

#### Public API
tbd.

#### Mobile API
tbd.


### API Gateways and Portals
Our believe in the inherent adaptability and robustness of a loosly coupled and decentralized service landscape and infrastructure also applies to API Management (APIm). There is no central APIm instance in Haufe, but a set of APIm instances servicing distinct groups of contextual related API's. Keeping with the spirit of 'how the web is working', we provide a central API search engine (like Google) to find APIs across Haufe.

The introduction of a new API management solution has to be discussed with the CTO office and is subject to review by the Architecture Review Board (ARB).

We encourage that also the API management solution, or at least, the API serviced by the APIm solution, should be maintained and operated (in the DevOps sense) by the team which operates and maintains the underlying service. The API inside the API management solution is to be treated **as part of the service, it is not an addon**.

Please adhere to the following naming schema:

* `<topic hub>.haufe.io` points to the developer portal of a topic's API management solution. This is the URL the developers will use at design time of the consuming application
* `api.<topic hub>.haufe.io` points to the API gateway of the topic's API management solution. This is the URL the consuming clients will use at runtime.


#### Meta portal `apis.haufe.io`

(TBD, this is a suggestion, the API meta portal is not yet available)

In order to make all Haufe APIs searchable and discoverable, they have to be registered with the [`apis.haufe.io`](https://apis.haufe.io) search service.

The portal at `apis.haufe.io` does not provide any means of subscribing to an API, but just makes sure the APIs which can be located at various API portals (or just inside API gateways as may be the case for internal services) can be found and evaluated for their fitness for a specific purpose.

In order to achieve this kind of discoverability, it is mandatory for an API managed API to be registered with this API meta portal using its Swagger representation. **Swagge 2.0/OpenAPI is the minimum API documentation format**; a Swagger definition **must** be present, all other information which can and will be defined for an API is optional in terms of discoverability.

#### `haufe.io` Certificates

For the `*.haufe.io` domain, there exists a wildcard certificate which can be used for the developer portals.

For `api.<topic hub.haufe.io` DNS entries, specific certificates have to purchased (via IT ticket). Currently, we do not have an automated process for this, but in those cases where the API gateway is operated by ourselves, setting up [Let's Encrypt](https://letsencrypt.org) is a viable solution which is encouraged, wherever it is feasible.

#### Service Hub

The Service hub is located at the following URLs:

* Developer Portal: [`https://servicehub.haufe.io`](https://servicehub.haufe.io)

The API gateway is addressed with `https://api.servicehub.haufe.io`, which is also documented when opening the portal.

The Service Hub is intended for microservices which are not especially bound to a specific domain. Examples of types of services which can be found in the service are:

* Health Insurance public information
* Bank information (IBAN calculator, BIC directory)
* ...

Additional services for the service should fall into approximately the same size and topic corridor.

The Service Hub runs using the Azure API Management SaaS solution.

#### SSMP Hub

The SSMP hub contains APIs which are dealing with the Sales, Services and Marketing platform based on Salesforce. In some cases, we need an efficient and stable way of accessing and/or updating data in Salesforce, and those APIs are located inside the SSMPhub API Management portal.

The SSMP hub is located at the following URL:

* [`https://ssmphub.haufe.io`](https://ssmphub.haufe.io)

Likewise, the API gateway (which in turn can be looked up in the API gateway) is located at the URL `https://api.ssmphub.haufe.io`.

The SSMP Hub runs using the Azure API Management SaaS solution.

#### Content Hub

The Content Hub contains all APIs which are content related: Ingestion of new content, search and retrieval of content. The APIs which are content related are located at the following location:

* [`https://contenthub.haufe.io`](https://contenthub.haufe.io)

Likewise, the API gateway (which in turn can be looked up in the API gateway) is located at the URL `https://api.ssmphub.haufe.io`.

As the Service Hub and the SSMP Hub, additional content driven services may be subject to grouping inside the Content Hub API Management solution.

The Content Hub runs using the Azure API Management SaaS solution.

### The `Forwarded` header

Whenever the API Gateway makes a call to a backend API service, it MUST add a `Forwarded` header (see [RFC 7239](https://tools.ietf.org/html/rfc7239)), which contains the "front end" visible host and URL data which can subsequently be used to assemble [hypermedia](response-format.md) links.

See also [Hypermedia and REST](hypermedia-and-rest.md) for a discussion of this header.

#### Implementation in Azure API Management

By applying a custom policy on the Product or even Tenant level, you can insert this header automatically:

```
<inbound>
	<set-header name="Forwarded" exists-action="override">
		<value>@("proto=" + context.Request.OriginalUrl.Scheme + ";host=" + context.Request.OriginalUrl.Host + ";prefix=" + context.Api.Path)</value>
	</set-header>
	<base />
</inbound>
```

Example: Your end point is called at `https://api.contenthub.haufe.de/search/v1/query?q=Steuerhinterziehung`. The API prefix is `/search/v1`, and the operation is `/query`. In this case, the header will look as follows:

```
Forwarded: proto=https;host=api.contenthub.haufe.de;prefix=/search/v1
```

The use of `port=` is optional, but MUST be used in case the port is not the standard `443` port. The port MUST NOT be part of the `host=` statement.

#### Implementation using the wicked API Portal

In `config.json` of an API:

```
  "plugins": [
      {
          "name":"request-transformer",
          "config":{
              "add": {
                  "headers": [
                      "%%Forwarded"
                  ]
              }
          }
      }
  ]
```

The special string `%%Forwarded` will dynamically be replaced with a suitable `Forwarded: ...` header for the specific API, API Host and schema.

### Automating API deployments

As an integral part of a web (http(s) based) service, the API Management bits and pieces also need to be a part of the deployment process.

#### Blue/Green Deployment - Backend Service Level
tbd.

#### Blue/Green Deployment - API Management Level
tbd.