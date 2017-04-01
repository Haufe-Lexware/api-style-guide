## Security and Authentication

### Authentication

OAuth 2.0 is the de-facto standard for API security. OpenId Connect is a simple identity layer on top of the OAuth 2.0 authorization framework to enable Clients to verify the identify of the End-User. This means that Web or mobile apps exposing APIs don’t have to share passwords. It allows the API provider to revoke tokens for an individual user for an entire app, without requiring the user to change their original password. This is critical, if a mobile device is compromised or if a rogue app is discovered.

Overall, OAuth 2.0 will mean improved security by dropping the use of signatures and cryptography and relies on TLS for securing data in transit, which makes it transport dependent. This leads to a better end-user and consumer experiences with Web and mobile apps.

Don't do something *like* OAuth, but different. It will be frustrating for app developers if they can't use an OAuth library in their language because of your variation.

For more information on Authentication and Authorization see the section on [API Management](../api-management/api-management.md); it contains information on specific scenarios where certain OAuth2.0 flows can and MUST be applied. API Management solutions can help implementing these scenarios in a more generic and decoupled way.

### SSL everywhere - All the time

Always use SSL. No exceptions. Today, your web APIs can get accessed from anywhere where internet is given (like libraries, coffee shops, airports among others). Not all of these access points are secure. Many don't encrypt communications at all, allowing for easy eavesdropping or impersonation if authentication credentials are hijacked.

Another advantage of always using SSL is that guaranteed encrypted communications simplifies authentication efforts - you can get away with simple access tokens instead of having to sign each API request.

One thing to watch out for is non-SSL access to API URLs. Do not redirect these to their SSL counterparts. Throw a hard error instead! The last thing you want is for poorly configured clients to send requests to an unencrypted endpoint, just to be silently redirected to the actual encrypted endpoint.
