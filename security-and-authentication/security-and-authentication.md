##Security and Authentication

###Authentication

Use the latest and greatest OAuth 2.0 or - for social login - the OpenID Connect standards. It means that Web or mobile apps that expose APIs don’t have to share passwords. It allows the API provider to revoke tokens for an individual user, for an entire app, without requiring the user to change their original password. This is critical if a mobile device is compromised or if a rogue app is discovered.

Above all, OAuth 2.0 will mean improved security and better end-user and consumer experiences with Web and mobile apps.

Don't do something *like* OAuth, but different. It will be frustrating for app developers if they can't use an OAuth library in their language because of your variation.

### SSL everywhere - All the time

Always use SSL.  No exceptions.  Today, your web APIs can get accessed from anywhere there is internet (like libraries, coffee shops, airports among others).  Not all of these are secure. Many don't encrypt communications at all, allowing for easy eavesdropping or impersonation if authentication credentials are hijacked.

Another advantage of always using SSL is that guaranteed encrypted communications simplifies authentication efforts - you can get away with simple access tokens instead of having to sign each API request.

One thing to watch out for is non-SSL access to API URLs. Do not redirect these to their SSL counterparts. Throw a hard error instead!  The last thing you want is for poorly configured clients to send requests to an unencrypted endpoint, just to be silently redirected to the actual encrypted endpoint.
