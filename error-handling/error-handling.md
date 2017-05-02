## Error handling

Just like an HTML error page shows a useful error message to a visitor, an API should provide a useful error message in a known consumable format. The representation of an error should be no different than the representation of any resource, just with its own set of fields.

The API should always return sensible HTTP status codes.  API errors typically break down into 2 types: `4xx` series status codes for client issues & `5xx` series status codes for server issues. At a minimum, the API should standardize that all `4xx` series errors come with consumable JSON error representation. If possible (i.e. if load balancers & reverse proxies can create custom error bodies), this should extend to `5xx` series status codes.

A JSON error body should provide a few things for the developer - a useful error message, a unique error type (that can be looked up for more details in the docs), possibly a detailed description, and an optional instance identifier that makes it possible to refer to this particular occurrence of the problem. (Typically, the instance identifier enables the service operators to look up the internal service logs that were written when the problem was detetcted.) Examples of JSON output representation for something like this would look like:

	{   
	   "code": 1234,   
	   "message": "Something bad happened :(",
	   "description": "More details about the error here",
	   "logref": "7673868d-231e-490d-9c4f-19288e7e668d"
	}

or

	{  
	   "type": "http://your.service.namespace/error/types/3245",
	   "message": "Something bad happened :(",
	   "description": "More details about the error here",
	   "instance": "urn:uuid:7673868d-231e-490d-9c4f-19288e7e668d"
	}


Validation errors for PUT, PATCH and POST requests will need a field breakdown. This is best modeled by using a fixed top-level error code for validation failures and providing the detailed errors in an additional errors field, like so:

	{   
	   "code": 1024,
	   "message": "Validation Failed",
	   "errors":
		 [
	     {
	       "code": 5432,
	       "field": "first_name",
	       "message": "First name cannot have fancy characters"
	     },
	     {
	        "code": 5622,
	        "field": "password",
	        "message": "Password cannot be blank"
	     }
	   ],
	   "logref": "7673868d-231e-490d-9c4f-19288e7e668d"
	}

or

	{  
	   "type": "http://your.service.namespace/error/types/1024",
	   "message": "Validation Failed",
	   "errors":
		 [
	     {
	       "type": "http://your.service.namespace/error/types/5432",
	       "field": "first_name",
	       "message": "First name cannot have fancy characters"
	     },
	     {
	        "type": "http://your.service.namespace/error/types/5622",
	        "field": "password",
	        "message": "Password cannot be blank"
	     }
	   ]
	   "instance": "urn:uuid:7673868d-231e-490d-9c4f-19288e7e668d"
	}

> :information_source: Thanks to Vinay Sahni for this description http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#errors.

### Error Response Format

The examples given above implement different [Hypermedia error response formats](http://nocarrier.co.uk/the-error-hypermedia-type/):

* [vnd.error draft](https://github.com/blongden/vnd.error)
* [RFC 7807](https://tools.ietf.org/html/rfc7807): Problem Details for HTTP APIs

The latter has the advantage (besides not being a draft anymore) that it defines an equivalent content type for XML-based APIs.

Be aware that for security sensitive APIs like User Management the need to return meaningful errors is always in tension with not returning too much information to guide a potential attacker. Dictionary attacks can be devastatingly effective, if for instance the exact makeup of the password policy is revealed in the response.

At the same time, nothing is more frustrating for a customer to try to create a matching password without knowing the password constrains. This is why in general one would only show the password policy during initial setup and during password change, but not in response to login failures. During login you would never distinguish between a wrong username or a right username and a wrong password. Both should return the exact same error as to not allow a more targeted attack. (Note: This is why there is usually only a single link to cover both forgotten passwords and usernames).

When using either of the above mentioned error formats [vnd.error](https://github.com/blongden/vnd.error) or [RFC 7807](https://tools.ietf.org/html/rfc7807), we can indicate an error and provide a ‘help’ link with a human-readable description of the error. A client (with the appropriate sensibility to the security issues I describe above) can then decide to render the help link oder omit it. In this way the error description is decoupled from the actual client. Changes to the underlying  business logic (ie. password policy) and corresponding human readable error descripton are entirely contained within the API provider without impacting any existing API clients.
