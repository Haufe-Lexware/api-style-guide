##Collection Resources
A list of all of the given resources, including any related metadata. Array of resources should be in the `_embedded` field. Fields like `total_items` and `total_pages` help provide context to paged results. Consistent naming of collection resource fields allow API clients to create generic handling for using the provided data across various resource collections.

The GET verb should not effect the system, and should not change response on subsequent requests (unless the underlying data changes), i.e. it should be idempotent. Exceptions to 'changing the system' are typically instrumentation/logging-related.

The list of data is presumed to be filtered based on the provided security context of the API client, this should not be a list of all resources in the domain.

Providing a summarized, or minimized version of the data representation can reduce the bandwidth footprint, in cases where individual resources contain a large object.

###Resource naming

Collection resource names should be plural nouns, e.g. /users. This helps visually disambiguate collections from singletons.
Please have a look at [REST principles](../rest-principles/rest-principles.md) for naming Guidelines.

###Get List of resources

#####URI Template

	GET /{namespace}/{version}/{resource}
 

#####Example Request

	GET /user_management/v1/users
 

#####Example Response

HAL response format

	{
	  "_links": {
	    "self": { "href": "{baseurl}/users"},
	    "first": { "href": "{baseurl}/users?page=1"},
	    "last": { "href": "{baseurl}/users?page=11"},
	    "next": { "href": "{baseurl}/users?page=2"},
	    "find": { "href": "{baseurl}/users{?id}", "templated": true}
	  }
	  "page": 1,
	  "page_size": 20,
	  "total_pages" : 11,
	  "total_count": 217,
	  "_embedded": {
	    "users": [
	      {
	        "_links": {
	          "self": { "href": "{baseurl}/users/A14DA7707FE2458DAE37C2CF81E8F9B1" }
	        },
	        "id" : "A14DA7707FE2458DAE37C2CF81E8F9B1",
	        "name" : "Mustermann"
	      },
	      {
	        "_links": {
	          "self": { "href": "{baseurl}/users/BE14A7269802498F992813885546D058" }
	        },
	        "id" : "BE14A7269802498F992813885546D058",
	        "name" : "VIPUser"      
	      },
	    ]
	  }
	}
 


####HTTP Status

If the collection is empty (0 items in response), `404 Not Found` is not appropriate. The corresponding array should just be empty, and collection metadata fields provided (e.g. `"total_count": 0`). Invalid query parameter values can result in 400 Bad Request. Otherwise `200 OK` is utilized for a successful response.

###Read Single Resource

A single resource, typically derived from the parent collection of resources (often more detailed than the collection resource items).

Executing GET should never affect the system, and should not change response on subsequent requests, i.e. it should be idempotent.

> All identifiers for sensible resources (customers, individuals) should be non-sequential, and preferrably non-numeric.

In scenarios where this data might be used as a subordinate to other data, immutable string identifiers should be utilized for easier readability and debugging (i.e. "NAME_OF_VALUE" vs 1421321).

#####URI Template

	GET /{namespace}/{version}/{resource}/{resource-id}
 

#####Example Request

	GET /user_management/v1/users/BE14A7269802498F992813885546D058
 

#####Example Response

	{
	  "_links": {
	    "self": { "href": "{baseurl}/users/BE14A7269802498F992813885546D058"},
	  }
	  "id": "BE14A7269802498F992813885546D058",
	  "name": "Mustermann"
	}
 

#####HTTP Status

If the provided resource identifier is not found, responds `404 Not Found` HTTP status. Otherwise, `200 OK` HTTP status should be utilized when data is found.

###Update Single Resource

Updates a single resource. The shape of the PUT request should maintain parity with the GET response for the selected resource. Fields in the request body can be optional or ignored during deserialization, such as "create_time" or other system-calculated values.

#####URI Template

	PUT /{namespace}/{version}/{resource}/{resource-id}
 

#####Example Request

	PUT /user_management/v1/users/BE14A7269802498F992813885546D058
	{ 
	  "id": "BE14A7269802498F992813885546D058",
	  "name": "Changed Name"
	}
	 

#####HTTP Status

Any failed request validation responds `400 Bad Request` HTTP status. If clients attempt to modify read-only fields, this is also a `400 Bad Request`.

If there are business rules (more than data type/length/etc), it is best to provide a specific error code & message (in addition to the 400) for that validation.

For situations which require interaction with APIs or processes outside of the current request, the `422` status code is appropriate.

After successful update, PUT operations should respond with `204 No Content` status, with no response body.

###Update Partial Single Resource

#####Support of partial updates is optional.

Updates a part of a single resource. Unlike PUT, which requires parity with GET, PATCH merely changes the fields provided, and leaves the rest of the resource unaffected.

HTTP PATCH method has been been formalized in RFC 7386 JSON Merge Patch..

Response should be 204 No Content and no response body. Because PATCH is often called frequently in interactive form UX design, returning the entire response could be irresponsible from a bandwidth perspective, especially in mobile scenarios.

System generated values should be commonly changed by an update, 

#####URI Template

	PATCH /{namespace}/{version}/{resource}/{resource-id}
 

#####Example Request

	PATCH /user_management/v1/users/BE14A7269802498F992813885546D058 
	{ 
	  "name" : "newname"
	}
 

#####Example Response

	204 No Content

#####HTTP Status

Status/response for PATCH is the same as PUT.

###Delete Single Resource

Deletes a single resource. In order to enable retries (typically patchy connectivity), DELETE is treated as idempotent, so it should always respond with a '204 No Content' HTTP status.
'404 Not Found' HTTP status should not be utilized here, as on a second retry a client might mistakenly think the resource never existed at all. GET can be utilized to verify the resources exists prior to DELETE.

#####URI Template

	DELETE /{version}/{namespace}/{resource}/{resource-id}
 
#####Example Request

	DELETE /user_management/v1/users/BE14A7269802498F992813885546D058
 
#####Example Response

	204 No Content
 
###Create New Resource

Creates a new resource in the collection. Request body may be somewhat different than GET/PUT response/request (typically fewer fields as the server will generate some values).

In most cases, the API server produces an identifier for the resource. In cases where identifier is supplied by the API consumer, use [Create New Resource - Consumer Supplied Identifier](#create-new-resource---consumer-supplied-identifier) below.

Once the POST has successfully completed, a new resource will be created. The identifier for this resource should be added to the resource collection URI.

Hypermedia links provide an easy way to get the URL of the newly created resource, using the rel: self, in addition to other links for operations allowed for the new resource. Addionally a location header in the response can point to the newly created resource.

#####URI Template

	POST /{version}/{namespace}/{resource}
 
#####Example Request

Note that server-generated values are not provided in the request.

	POST /user_management/v1/users
	
	{
	    "name": "MyName123",
	}
 
#####Example Response

	201 Created 
	Location: http://api.haufe-lexware.com/user_management/v1/users/E75E30C0607446219C6EA31735C691B9
	
	{
	  "_links": {
	    "self": { "href": "{baseurl}/users/E75E30C0607446219C6EA31735C691B9"},
	  }
	  "id": "E75E30C0607446219C6EA31735C691B9",
	  "name": "MyName123"
	}
 
###Create New Resource - Consumer Supplied Identifier

When an API consumer defines the resource identifier, the PUT verb should be utilized, as the operation is idempotent, even during creation.

The same interaction as  [Create New Resource](#create-new-resource) is used here. 201 + response body on resource creation, and 204 + no response body when an existing resource is updated.

 
