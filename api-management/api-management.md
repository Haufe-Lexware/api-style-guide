## API Management

### Recommended API Management solutions

The recommended go-to API Management solution is our own API Management System [wicked.haufe.io](http://wicked.haufe.io) ([github](https://github.com/Haufe-Lexware/wicked.haufe.io)). It is based on [Mashape Kong](https://getkong.org) and provides both an API Gateway and a developer API Portal, implemented in node.js.

The wicked.haufe.io API Portal enables most common scenarios and is also developed further both by internal developers and (in the future hopefully) by contributions from external developers. Wicked has been open sourced under the Apache 2.0 License.

Some projects (especially those running on the Azure platform) have also successfully adopted Azure API Management. Azure API Management is a fully managed service, in contrast to wicked, which has to be operated by the team actually wanting to use it.

#### Azure API Management

Azure API Management is fully managed and is available in most Azure regions. It provides a stable API Gateway service and a functional, but not very flexible, API Portal.

The main advantage of Azure APIm is that it's managed; the main drawback is that it is not built with CI/CD in mind. Creating deployment pipelines and configuration as code for Azure APIm is challenging. There exist efforts made by Haufe to make this easier, such as the [azure-apim-deployment-utils](https://github.com/Haufe-Lexware/azure-apim-deployment-utils).

Running a production workload via Azure APIm requires the "Standard Tier" of Azure APIm, which can be quite costly (at around 500€/month) for smaller portals.

#### wicked.haufe.io

In contrast to the fully managed service Azure APIm, wicked.haufe.io is much more flexible in that it may run on any runtime which supports Docker (e.g. Docker Hosts, Docker Swarm, Kubernetes). But with flexibility also comes a price, and that's obviously that you need to operate it yourself.

A prerequisite to successfully running wicked.haufe.io in production is the availability of a stable docker runtime. If you have already established this, choosing wicked as an APIm solution is easy, as you have already solved the difficult problems.

A bonus on top of the flexibility of wicked is that it's built to tie in to deployment pipelines and to support CI/CD scenarios, also in terms of API Management, documentation and such. It's designed to support exactly those scenarios, the only offering to configuration is **configuration as code** (or in this case as `json`).

At the [Github site of wicked.haufe.io](https://github.com/Haufe-Lexware/wicked.haufe.io), you can find much more documentation on the system.

### API Management Scenarios

There are many different scenarions in which API management can and must be used. The following sections describe some basic scenarios.

The below sections are always to be read in the scope of APIs, not for  communication between systems which are tightly coupled (which is to be avoided, granted, by cannot always be).

#### Machine to Machine communication

Machine to machine communication in the API sense is when two systems/machines communicate with each other and where the machines trust each other. This is the case for batch jobs or system jobs running in the background, or, if a user identity (authentication) and role (authorization) has already been established, and the participating systems trust each other to have established this beforehand.

Machine to machine communication can be secured via the following means:

* Secure transport (TLS/https, **mandatory**)
* Using API Keys (as a header) to authenticate and authorize the client system with the provider system
* **or** using the [OAuth2.0 Client Credentials Flow](https://tools.ietf.org/html/rfc6749), where client credentials (client ID and client Secret) are exchanged for an access token

API Keys are directly supported by both Azure APIm and Wicked; the OAuth2.0 CC Flow is only supported out of the box by Wicked. TLS is provided by both. Wicked can be integrated with Let's Encrypt, Azure APIm cannot.

Using API Management and an API Portal for these scenarios is beneficial if you potentially have more than one client for an API, and you may want to track usage and know your clients and clients' behaviour.

#### API Usage with an end user context

In many cases you need to get a user context into the API call, i.e. the backend service needs to know on behalf of which actual user an API call is done.

The authentication and authorization of the user for the API should not be done by the actual API Backend implementation, but rather by the API Management/API Gateway in front of the implementation. By following this pattern, the API Backend can just trust the user identity and roles/rights and does not need to verify that type of information.

We propose that the [OAuth2.0 Standards](https://tools.ietf.org/html/rfc6749) MUST be used for standard scenarios in terms of authentication and authorization, except where the SAML standard is appropriate for legacy integrations (such as to Atlantic). The important thing we want to stress here is that the API backend implementation MUST be separated from the Authentication and Authorization, so that this aspect can be changed without having to change the entire backend implementation.

Instead, this is done in a decoupled way in a dedicated component: An Authorization Server. An Authorization Server has two distinct tasks:

* Authenticate the end user against an identity provider (this can be most anything, the Authorization Server may under special circumstances also implement the authentication itself; e.g. Haufe Atlantic, Google, Github, Facebook,...)
* Authorize the end user for the API Usage by deciding on "scopes" (approx. rights), depending on the identity/roles/properties of the user; this is highly project specific and can usually not be delegated to a generic implementation

After the user is authenticated and (possibly) authorized, an access token is issued to the API client, which then carries the information on user identity and scopes; how this is done technically is an implementation detail, but the main two techniques are:

* Using registered tokens which map to headers in the API Gateway
* Usign JWT tokens, which are cryptographically signed and carry the above information in itself (thus they need not be registered with the API Gateway)

On of the following standards MUST be used for authentication and authorization:

* **Authentication**: OAuth2.0 Authorization Code Flow, OAuth2.0 Resource Owner Password Grant Flow, OAuth 1.0a, or SAML 2.0 (e.g. Atlantic); in some cases also REST calls to establish identity can be used (equivalent to Resource Owner Password Grant flow, e.g. using an existing SAML SSO Token)
* **Authorization**: OAuth2.0 Authorization Code Flow, OAuth2.0 Resource Owner Password Grant Flow or OAuth2.0 Implicit Grant Flow

Which OAuth2.0 Flow can be applied depends on the type of API Client:

* Confidential Clients are able to keep secret/credentials confidential, i.e. server side. Traditional web sites are usually confidential clients.
* Public Clients cannot keep secrets/credentials confidential, as those would be kept in a place with the actual end user; typical public clients are mobile apps (can be reverse engineered) or single page applications (JavaScript applications which do not have a stateful backend)

##### End user context - Confidential Clients

Confidential Clients should use the OAuth2.0 Authorization Code grant to get access to APIs (in case the API supports this flow).

The Authorization Code grant has the following positive aspects:

* The Access Token needed to be able to call the API is never exposed to the end user, it is only transferred via the backend to the client application
* The end user's real credentials are never present inside the client application, only the access token is used to call the API on behalf of the logged in user
* Using the Authorization Code Flow also renders a refresh token which can be used to refresh a shortlived access token, without user interaction; this can greatly improve usability of the client application using the API

Under certain circumstances, for trusted applications, the Resource Owner Password Grant flow may also be used. This must never be the case if the API is used by a third party application (implemented by somebody else than Haufe). This flow will also return both an access token and a refresh token, but the end user's username and password will need to be processed (temporarily) inside the client application.

Wherever possible though, try to implement the Authorization Code Flow.

##### End user context - Public Clients

For non-confidential clients, such as Single Page Application (aka Public clients), the OAuth 2.0 Implicit Grant Flow MUST be used.

In order for the SPA/Public Client with a User Agent to retrieve an access token for API Access, the end user's User Agent (aka Browser) is redirected to the Authorization Server. The AS establishes identity (by a suitable means) and decides whether this particular user is granted access (authorized) to the API or not.

The AS then issues an Access Token (either by crafting a JWT or registering/getting an opaque token from the API Gateway) which is then returned to the calling SPA via a fragment (`https://spa.com/#access_token=....`).

This flow explicitly does NOT return a refresh token (which would need to be kept confidentially - not possible in an SPA), and the access token should also be very short lived (less than 24 hours, possibly only as little as 30 minutes).

#### Mobile Clients

Mobile Clients are a special case of public clients; some mobile app platforms support the use of User Agents (embedded browsers), which also in some case are able to re-use pre-established trust settings. For these cases, consider using the OAuth 2.0 Implicit Grant flow for retrieving access tokens for use in the mobile app.

In case you are writing a trusted type of mobile app (we are writing our own native iOS app or similar), the Resource Owner Password Grant can also be used, in the following way:

* The Mobile App collects username and password from the end user
* The Mobile App's client ID (stored inside the app; the secret must not be!) and the username/password are used to call an Authorization Server via the OAuth 2.0 Resource Owner Password Grant
* The Authorization Server verifies it knows the Mobile App's Client ID
* The Authorization Server verifies the username and password against a suitable Identity Provider
* The Authorization Server decides whether the authenticated user is granted access (can be authorized) or not
* In case of success, the Authorization Server returns and Access Token and a Refresh Token to the mobile app
* The Mobile App stores Access Token and Refresh Token in separate places inside the mobile devices storage; after that, it discards the username and password of the end user, so that this data is no longer present on the device (at least not in the context of this mobile app)
* The Mobile App can now access the API, and also refresh the Token using the Refresh Token.

This scenario may be implemented e.g. using the Wicked API Management system, using a custom Authorization Server implementation.

Please note that such an implementation should ensure that a refresh token is usable only once, and is invalidated after use. Ideally, the end user SHOULD also have a possibility to review access/refresh tokens in use, e.g. using a separate web interface.

**APIs without end user context**: Mobile Clients which want to use an API without actually having an end user context can still make use of a similar approach to getting access tokens to the API. By using a device ID or even a random number as client credentials, rate limiting and such can be applied to a specific device. The Authorization Server would then usually unconditionally accept the client's credentials and issue tokens. You would then still have the possibility to revoke or at least limit access to specific devices/IDs in your Authorization Server implementation.

Please note that this technique does not actually prevent misuse of your API, but it at least raises the bar for the attacker. Establishing real user identity is a more reliable means of preventing API misuse.

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

In order to achieve this kind of discoverability, it is mandatory for an API managed API to be registered with this API meta portal using its Swagger representation. **Swagger 2.0/OpenAPI is the minimum API documentation format**; a Swagger definition **must** be present, all other information which can and will be defined for an API is optional in terms of discoverability.

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

In most cases you can see the API Management as a special type of "Microservice", which has a separate deployment lifecycle than the actual services which are proxied behind them:

* A service release does not necessarily entrail a new release of the API Management portal/gateway, as most changes are transparently proxied to the backend
* Changed documentation does not necessarily require a change of the API implementation which the API Gateway proxies

There may be situations, especially if e.g. the Swagger definition is generated by a service build, where a documentation update may be automatically triggered after a service deployment, but this is an implementation detail of the API system. The most important thing to ensure is that both parts can be independently deployed, and that a service deployment never must break the API Management deployment, and vice versa (they must be decoupled).

Deployment pipelines of the services may e.g. contain a step which diffs a Swagger definition with the definition in the documentation portal and possibly automatically pushes a change there, which in turn is picked up by a different pipeline (the deployment pipeline/configuration change pipeline of the API Management system). This is quite easily implemented using Wicked, not so with Azure APIm.
