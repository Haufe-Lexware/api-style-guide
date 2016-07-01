##Error handling
Just like an HTML error page shows a useful error message to a visitor, an API should provide a useful error message in a known consumable format. The representation of an error should be no different than the representation of any resource, just with its own set of fields.

The API should always return sensible HTTP status codes.  API errors typically break down into 2 types: 400 series status codes for client issues & 500 series status codes for server issues. At a minimum, the API should standardize that all 400 series errors come with consumable JSON error representation.  If possible (i.e. if load balancers & reverse proxies can create custom error bodies), this should extend to 500 series status codes.

A JSON error body should provide a few things for the developer - a useful error message, a unique error code (that can be looked up for more details in the docs) and possibly a detailed description.  JSON output representation for something like this would look like:

	{   
	   "code" : 1234,   
	   "message" : "Something bad happened :(",
	   "description" : "More details about the error here" 
	} 
 

Validation errors for PUT, PATCH and POST requests will need a field breakdown. This is best modeled by using a fixed top-level error code for validation failures and providing the detailed errors in an additional errors field, like so:

	{   
	   "code" : 1024,
	   "message" : "Validation Failed",
	   "errors" : [
	     {
	       "code" : 5432,
	       "field" : "first_name",
	       "message" : "First name cannot have fancy characters"
	     },
	     {
	        "code" : 5622,
	        "field" : "password",
	        "message" : "Password cannot be blank"
	     }
	   ]
	} 
 
> :information_source: Thanks to Vinay Sahni for this description http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#errors.
 
### Error Response Format

An example for a [Hypermedia error response format](http://nocarrier.co.uk/the-error-hypermedia-type/) can be found on [here]( 
https://github.com/blongden/vnd.error).

Be aware that for security sensitive APIs like User Management the need to return meaningful errors is always in tension with not returning too much information to guide a potential attacker. Dictionary attacks can be devastatingly effective if for instance the exact makeup of the password policy is revealed in the response.

At the same time nothing is more frustrating for a customer to try to create a matching password without knowing the password constrains. This is why in general one would only show the password policy during initial setup and during password change, but not in response to login failures. During login you would never distinguish between a wrong username or a right username and a wrong password. Both should return the exact same error as to not allow a more targeted attack. (Note: This is why there is usually only a single link to cover both forgotten passwords and usernames).

Using the [error format](https://github.com/blongden/vnd.error) above we can indicate an error and provide a ‘help’ link with a human-readable description of the error. A client (with the appropriate sensibility to the security issues I describe above) can then decide to render the help link oder omit it. In this way the error description is decoupled from the actual client. Changes to the underlying  business logic (ie. password policy) and corresponding human readable error descripton are entirely contained within the API provider without impacting any existing API clients.

 
