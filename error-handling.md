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
 

 
