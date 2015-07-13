##Caution: this is not the final version but work in progress. Use with caution

- [Introduction](#introduction)
- [URI Components](#uri-components)
  - [Version](#version)
  - [Namespaces](#namespaces)
  - [Resource References](#resource-references)
- [Collection Resources](#collection-resources)
  - [Collection Resource](#collection-resource)
    - [Filtering](#filtering)
      - [Paging](#paging)
        - [Hypermedia links](#hypermedia-links)
      - [Time selection](#time-selection)
      - [Sorting](#sorting)
  - [Read Single Resource](#read-single-resource)
  - [Update Single Resource](#update-single-resource)
  - [Update Partial Single Resoure](#update-partial-single-resoure)
  - [Delete Single Resource](#delete-single-resource)
  - [Create New Resource](#create-new-resource)
  - [Create New Resource - Consumer Supplied Identifier](#create-new-resource---consumer-supplied-identifier)
- [Sub-Resource Collection](#sub-resource-collection)
    - [Cautions](#cautions)
- [Sub-Resource Singleton](#sub-resource-singleton)
- [Complex Operation](#complex-operation)
  - [Risks](#risks)
  - [Benefits](#benefits)
  - [Resource-Oriented Alternative](#resource-oriented-alternative)
  - [Complex Operation - Sub-Resource](#complex-operation---sub-resource)
  - [Complex Operation - Composite](#complex-operation---composite)
  - [Complex Operation - Transient](#complex-operation---transient)
  - [Complex Operation - Search](#complex-operation---search)
- [Read-only resources](#read-only-resources)

## Introduction

The purpose of this style guide is to gather a list of rules, best practices, resources and our way of creating REST APIs in Haufe Group.
The style guide focuses on REST APIs cause this is the preferred way to expose APIs to our services.
The style guide is work in progress. We’d love your feedback – whether you agree, disagree, or have some additional practices and tips to add.

When creating API capabilities, it's important to respect existing HTTP interaction
patterns and resource models that we have utilized across the platform. If the interaction pattern you are planning on doesn't fit into these patterns, please consult with an API designer.

This is intended to augment the more comprehensive API Standards. If any conflict exists in guidance, API Standards should trump any guidance here. If conflicts are found, please contact API Design.

## URI Components

### Version

The URI should include `/vN` with the major version (`N`) as a prefix. 

URL-based versioning is utilized for it's simplicity of use for API consumers, versus the more complex header-based approach.

#### URI Template
	/v{version}/

##### Example
	/v1/

### Namespaces

In any URI, the first noun (which may be singular or plural, depending on the situation) should be considered a “namespace”. Namespaces should reflect the customer's perspective on how the product works, not necessarily the company's hierarchy. 

#### URI Template
	/{version}/{namespace}/

#### Example
	/v1/vault/

### Resource References
The URI references for resources should consistenly use the same path components to refer to resources. Sub-namespace or sub-folders should be avoided, to maintain path consistency. This allows consumer developers to have a predictable experience in case they are building URIs in code.

#### URI Template
	/{version}/{namespace}/{resource}/{resource-id}/{sub-resource}/{sub-resource-id}


## Collection Resources

Data resources which intend to support some aspect of typical CRUD (Create, Read, Update, Delete) functionality. CRUD resources should be implemented with adherence to POST/GET/PUT/PATCH/DELETE HTTP verbs.

Collection resource names should be plural nouns, e.g. `/users`. This helps visually disambiguate collections from singletons.

| Verb | Usage | Idempotent | Notes |
| ---  | ---   | ---        |	---	 |
| GET   | Read  | X          |	|
| POST  | Create|            | Only idempotent with use of [PayPal-Request-Id](https://developer.paypal.com/webapps/developer/docs/api/#authentication--headers) header |
| PUT   | Create|            | Only used for creation when client provides the resource identifier  |
| PUT   | Update| X           | Only for entire resource update, not partial |
| PATCH | Update|            | Used with [JSON Patch](https://tools.ietf.org/html/rfc6902) message format |
| DELETE| Delete|            | Should be repeatable with positive response. See [Delete Single Resource](#delete-single-resource) for more detail |

### Collection Resource

A list of all of the given resources, including any related metadata. Array of resources should be in the `items` field. Fields like `total_items` and `total_pages` help provide context to paged results. Consistent naming of collection resource fields allow API clients to create generic handling for using the provided data across various resource collections.

The GET verb should not effect the system, and should not change response on subsequent requests (unless the underlying data changes), i.e. it should be idempotent. Exceptions to 'changing the system' are typically instrumentation/logging-related.

The list of data is presumed to be filtered based on the provided security context of the API client, this should not be a list of all resources in the domain.

Providing a summarized, or minimized version of the data representation can
reduce the bandwidth footprint, in cases where individual resources contain a large object.

#### Filtering
##### Paging
Pages of results should be referred to consistently by the query parameters `page` and `page_size`, where `page_size` refers to the amount of results per request, and `page` refers to the requested page.

Additionally, responses should include `total_items` and `total_pages` whenever possible, where `total_items` indicates the total items in the requested collection, and `total_pages` is the number of pages (iterpolated from `total_items`/`page_size`).

###### Hypermedia links
Hypermedia links are high value in navigating paged resource collections, as `page`/`page_size` query parameters can be maintained while navigating pages of results.

Links should be provided with rels of `next`, `previous`, `first`, `last` wherever appropriate.

##### Time selection
`start_time` or `{property_name}_after`, `end_time` or `{property_name}_before` query parameters shoudl be provided if time selection is needed

##### Sorting
`sort_by` and `sort_order` can be provided to allow for collection results to be sorted. `sort_by` should be a field in the individual resources, and `sort_order` should be `asc` or `desc`.

#### URI Template
	GET /{version}/{namespace}/{resource}
#### Example Request
	GET /v1/vault/credit-cards
#### Example Resopnse
~~~
{
    "total_items": 1,
    "total_pages": 1,
    "items": [
        {
            "id": "CARD-1SV265177X389440GKLJZIYY",
            "state": "ok",
            "payer_id": "user12345",
            "type": "visa",
            "number": "xxxxxxxxxxxx0331",
            "expire_month": "11",
            "expire_year": "2018",
            "first_name": "Joe",
            "last_name": "Shopper",
            "valid_until": "2017-01-12T00:00:00Z",
            "create_time": "2014-01-13T07:23:15Z",
            "update_time": "2014-01-13T07:23:15Z",
            "links": [
                {
                    "href": "https://api.sandbox.paypal.com/v1/vault/credit-cards/CARD-1SV265177X389440GKLJZIYY",
                    "rel": "self",
                    "method": "GET"
                },
                {
                    "href": "https://api.sandbox.paypal.com/v1/vault/credit-cards/CARD-1SV265177X389440GKLJZIYY",
                    "rel": "delete",
                    "method": "DELETE"
                },
                {
                    "href": "https://api.sandbox.paypal.com/v1/vault/credit-cards/CARD-1SV265177X389440GKLJZIYY",
                    "rel": "patch",
                    "method": "PATCH"
                }
            ]
        }
    ],
    "links": [
        {
            "href": "https://api.sandbox.paypal.com/v1/vault/credit-cards/?page_size=10&sort_by=create_time&sort_order=asc",
            "rel": "first",
            "method": "GET"
        }
    ]
}
~~~


#### HTTP Status
If the collection is empty (0 items in response), `404 Not Found` is not appropriate. The `items` array should just be empty, and collection metadata fields provided (e.g. `"total_count": 0`). Invalid query parameter values can result in `400 Bad Request`. Otherwise `200 OK` is utilized for a successful response.


### Read Single Resource
A single resource, typically derived from the parent collection of resources (often more detailed than the collection resource items).

Executing GET should never affect the system, and should not change response on subsequent
requests, i.e. it should be idempotent.

All identifiers for sensitive data should be non-sequential, and preferrably non-numeric. In scenarios where this data might be used as a subordinate to other data, immutable string identifiers should be utilized for easier readability and debugging (i.e. "NAME_OF_VALUE" vs 1421321).

#### URI Template
	GET /{version}/{namespace}/{resource}/{resource-id}
#### Example Request
	GET /v1/vault/customers/CUSTOMER-66W27667YB813414MKQ4AKDY
##### Example Response
	{
		"merchant_customer_id": "merchant-1",
        "merchant_id": "target",
        "create_time": "2014-10-10T16:10:55Z",
        "update_time": "2014-10-10T16:10:55Z",
        "first_name": "Kartik",
        "last_name": "Hattangadi"
	}

#### HTTP Status

If the provided resource identifier is not found, responds `404 Not Found` HTTP status (even with ’soft deleted’ records in data sources). Otherwise, `200 OK` HTTP status should be utilized when data is found.


### Update Single Resource

Updates a single resource. The shape of the PUT request should maintain parity with the GET response for the selected resource. Fields in the request body can be
optional or ignored during deserialization, such as "create_time" or other system-calculated values.

#### URI Template
	PUT /{version}/{namespace}/{resource}/{resource-id}
#### Example Request
	PUT /v1/vault/customers/CUSTOMER-66W27667YB813414MKQ4AKDY
	{
		"merchant_customer_id": "merchant-1",
        "merchant_id": "target",
        "create_time": "2014-10-10T16:10:55Z",
        "update_time": "2014-10-10T16:10:55Z",
        "first_name": "Kartik",
        "last_name": "Hattangadi"
	}
	
#### HTTP Status

Any failed request validation responds `400 Bad Request` HTTP status. If clients attempt to modify read-only fields, this is also a `400 Bad Request`.

If there are business rules (more than data type/length/etc), it is best to provide a specific
error code & message (in addition to the `400`) for that validation.

For situations which require interaction with APIs or processes outside of the current request, the `422` status code is appropriate. For more practical examples, see the [PPaaS Blog on this topic](http://ppaas/news/2014/05/01/added-standards-for-422-http-status/)

After successful update, PUT operations should respond with `204 No Content` status, with no response body.


### Update Partial Single Resoure
Updates a part of a single resource. Unlike PUT, which requires parity with GET, PATCH merely changes the fields provided, and leaves the rest of the resource unaffected. 

[JSON Patch](https://tools.ietf.org/html/rfc6902) is a message format used to execute ordered operations, used for all PATCH operations at PayPal.

Unless it optimizes client flow in calling other APIs, or system generated values could be commonly changed by an update, response should be `204 No Content` and no response body. Because PATCH is often called frequently in interactive form UX design, returning the entire response could be irresponsible from a bandwidth perspective, especially in mobile scenarios.

#### URI Template
	PATCH /{version}/{namespace}/{resource}/{resource-id}
#### Example Request
	PATCH /v1/notifications/webhooks/52Y53119KP6130839
	[
    {
        "op": "replace",
        "path": "/url",
        "value": "https://www.yeowza.com/paypal_webhook_new_url"
    }
#### Example Response
	204 No Content
    
#### HTTP Status
Status/response for PATCH is the same as PUT.

### Delete Single Resource
Deletes a single resource. In order to enable retries (typically patchy connectivity), DELETE is treated as idempotent, so it should always respond with a `204 No Content` HTTP status. `404 Not Found` HTTP status should not be utilized here, as on a second retry a client might mistakenly think the resource never existed at all. GET can be utilized to verify the resources exists prior to DELETE.

#### URI Template
	DELETE /{version}/{namespace}/{resource}/{resource-id}
#### Example Request
	DELETE /v1/vault/customers/CUSTOMER-66W27667YB813414MKQ4AKDY
	204 No Content

### Create New Resource

Creates a new resource in the collection. Request body may be somewhat different than GET/PUT response/request (typically fewer fields as the server will generate some values).

In most cases, the API server produces an identifier for the resource. In cases where identifier is supplied by the API consumer, use [Create New Resource - Consumer ID]()

Once the POST has successfully completed, a new resource will be created. The identifier for this resource should be added to the resource collection URI.

Hypermedia links provide an easy way to get the URL of the newly created resource, using the `rel`: `self`, in addition to other links for operations allowed for the new resource.

#### URI Template
	POST /{version}/{namespace}/{resource}
#### Example Request
Note that server-generated values are not provided in the request.

~~~
POST /v1/vault/credit-cards
{
    "payer_id": "user12345",
    "type": "visa",
    "number": "4417119669820331",
    "expire_month": "11",
    "expire_year": "2018",
    "first_name": "Betsy",
    "last_name": "Buyer",
    "billing_address": {
        "line1": "111 First Street",
        "city": "Saratoga",
        "country_code": "US",
        "state": "CA",
        "postal_code": "95070"
    }
}
~~~

#### Example Response
~~~
201 Created
{
    "id": "CARD-1MD19612EW4364010KGFNJQI",
    "valid_until": "2016-05-07T00:00:00Z",
    "state": "ok",
    "payer_id": "user12345",
    "type": "visa",
    "number": "xxxxxxxxxxxx0331",
    "expire_month": "11",
    "expire_year": "2018",
    "first_name": "Betsy",
    "last_name": "Buyer",
    "links": [
        {
            "href": "https://api.sandbox.paypal.com/v1/vault/credit-cards/CARD-1MD19612EW4364010KGFNJQI",
            "rel": "self",
            "method": "GET"
        },
        {
            "href": "https://api.sandbox.paypal.com/v1/vault/credit-cards/CARD-1MD19612EW4364010KGFNJQI",
            "rel": "delete",
            "method": "DELETE"
        }
    ]
}
~~~

### Create New Resource - Consumer Supplied Identifier

When an API consumer defines the resource identifier, the PUT verb should be utilized, as the operation is idempotent, even during creation.

The same interaction as [Create New Resource](#create-new-resource) is used here. 201 + response body on resource creattion, and 204 + no response body when an existing resource is updated.

## Sub-Resource Collection

Sometimes, multiple identifiers are required ('composite keys', in the database lexicon) to identify a given resource. In these scenarios, all behaviors of a [Collection Resource](collection-resource) are implemented, as a subordinate of another resource. It is always implied that the `resource-id` in the URL must be the parent of the sub-resources.

#### Cautions
* The need to maintain multiple identifiers can create a burden on client developers.
  * Look for opportunities to promote resources with unique identifiers (i.e. there is no need to identify the parent resource) to a first-level resource.
* Caution should be used in identifying the name of the sub-resource, as to not interfere with the identifier naming conventions of the base resource. In other words, `/{version}/{namespace}/{resource}/{resource-id}/{sub-resource-id}` is not appropriate, as the `sub-resource-id` has ambiguous meaning.
* Two levels is a practical limit for resource identifiers
  * API client usability suffers, as the need for clients to maintain state about identifier hierarchy increases complexity.
  * Server developers must validate each level of identifiers in order to verify that they are allowed access, and that they relate to each other, thus increasing risk and complexity.

Note these templates/examples are brief: for more detail on the Resource Collection style, [see above](crud/collection-resources). Although this section explains the sub-resource collection, all interactions should be the same, simply with the addition of a parent identifier.

#### URI Templates
	POST /{version}/{namespace}/{resource}/{resource-id}/{sub-resource}
    GET /{version}/{namespace}/{resource}/{resource-id}/{sub-resource}
    GET /{version}/{namespace}/{resource}/{resource-id}/{sub-resource}/{sub-resource-id}
    PUT /{version}/{namespace}/{resource}/{resource-id}/{sub-resource}/{sub-resource-id}
    DELETE /{version}/{namespace}/{resource}/{resource-id}/{sub-resource}/{sub-resource-id}

#### Examples
	GET /v1/notifications/webhooks/{webhook-id}/event-types
	POST /v1/factory/widgets/PART-4312/sub-assemblies
	GET /v1/factory/widgets/PART-4312/sub-assemblies/INNER_COG
	PUT /v1/factory/widgets/PART-4312/sub-assemblies/INNER_COG
	DELETE /v1/factory/widgets/PART-4312/sub-assemblies/INNER_COG

## Sub-Resource Singleton
This approach is usually used as a means to reduce the size of a resource, when use cases support the segmentation of a large resource into smaller resources. In scenarios where a resource has a one-to-one relationship to a single resource, representing that relationship will look different than a collection. 

When a sub-resource is a one-to-one relationship with the parent resource, the name should be a singluar noun. As often as possible, that single resource should always be present (i.e. does not respond with 404).

The sub-resource should be owned by the parent resource; otherwise this sub-resource should probably be promoted to it's own resource collection, and relationships represented with sub-resource collections in the other direction. Sub-resource singletons should not duplicate a resource from another collection.

If the singleton sub-resource needs to be created, PUT should be used, as the operation is idempotent, on creation or update. PATCH can be used for partial updates, but should not be available on creation (in part because it is not idempotent).

This should not be used as a mechanism to update single or subsets of fields with PUT. The resource should remain intact, and PATCH should be utilized for partial update. Creating sub-resource singletons for each use case of updates is not a scaleable design approach, as many endpoints could result long-term.

#### URI Template
	GET/PUT /{version}/{namespace}/{resource}/{resource-id}/{sub-resource}

#### Examples
	GET /v1/customers/devices/DEV-FDU233FDSE213f)/vendor-information

## Complex Operation

#### URI Template
~~~
POST /{namespace}/{action-resource}
~~~
*NOTE: Use with caution*

Also known as 'controller', or 'actions', this should be used with extreme prudence, only when [Resource Collection](#resource-collection) designs have been deliberately considered and disqualified. For further reading on 'controller' concepts, please refer to [section 2.6 of the RESTful Web Services Cookbook](http://www.amazon.com/RESTful-Web-Services-Cookbook-Scalability/dp/0596801688).

Complex operations should always be initiated with POST. The action should be indicated with a verb, and be singular in most cases.

Verbs such as 'activate', 'cancel', 'validate', 'accept', and 'deny' are usual suspects.

As a root URI (directly under namespace), this is usually an anti-pattern. This is typically only applicable for creating a variety of resources in an optimized operation.
In most cases, actions are taken on resources, and complex operations should be within a [Sub-Resource](#sub-resource-complex-operation).

When a use case is uniquely focused on actions, and not on resources, POST should always be used, and a singular verb should indicate the action to be performed. This helps to visually distinguish actions from [Collection Resources](#collection-resource).

### Risks
* Design scalability
	* When overused, the number of URIs can grow very quickly, as all permutations of root-level action can increase rapidly over time. This can also produce configuration complexity for routing/externalization.
	* The URI cannot be extended past the action, which precludes any possibility of sub-resources.
* Testability: highly compromised in comparison to [Resource Collection](#resource-collection)-oriented designs (due the lack of corresponding GET/read operations).
* History: the ability to retrieve history for the given actions is forced to live in another resource (e.g. `/action-resource-history`), or not at all.

### Benefits
* Avoids corrupting resource collection model with transient data (e.g. comments on state changes etc).
* Usability improvement: there are cases where a complex operation simplifies client interaction, where the client does not benefit from resource retrieval.


### Resource-Oriented Alternative
A better pattern is to create a [Resource Collection](#resource-collection) of actions and provide a history of those actions taken in `GET /{actions}`. This allows for future expansion of use cases around a resource model, instead of a single action-oriented, RPC-style URL. 

Additionally, for various use cases, filtering the resource collection of historical actions is usually desirable. This also feeds well into [event sourcing](http://martinfowler.com/eaaDev/EventSourcing.html) concepts, where the history of a given event can drive further functionality.

#### URI Template
    POST /{version}/{namespace}/{action}
#### Example Request
~~~
POST /v1/risk/payment-decisions
{
	"code": "h43j5k6iop"
}
~~~
#### Example Response
~~~
201 Created
{
    "code": "h43j5k6iop",
    "status": "APPROVED",
    "links": [
        {
            "href": "https://api.sandbox.paypal.com/v1/risk/payment-decisions/ID-FEF8EWR8E9FW)",
            "rel": "self",
            "method": "GET"
        }
    ]
}
~~~

### Complex Operation - Sub-Resource

*NOTE: Use with caution*

*For associated risks, see [Complex Operation](#complex-operation) above*

There are often situations in which a canonical resource needs to impart
certain actions or state changes which are not appropriate in a PUT or PATCH. These
URIs look like other Sub-Resources, but imply action.

A great use for this pattern is when a particular state change requires a "comment" (e.g. cancellation "reason"). Adding this comment, or other data such as location, would make the  GET/PUT unnecessarily include those extra fields on every request/response. This action may change the status of the given resource implicitly.

Additionally, when a resource identifier is required for an action, it's best to keep it in the URL. Some actions are business processes which are not innately a resource (and in some cases might not even change resource state). 

The response is typically `200 OK` and the resource itself, if there are expected to be changes in the resource the consumer needs to capture. However, if no resource state change occurs, `204 No Content` and no response body could also be considered appropriate.

#### URI Template
	POST /{version}/{namespace}/{resource}/{resource-id}/{complex-operation}

###### Example Request
~~~
POST /v1/payments/billing-agreements/I-0LN988D3JACS/suspend
{
    "note": "Suspending the agreement."
}
~~~

###### Example Response
~~~
204 No Content
~~~  
  
However, when state changes are imparted in this manner, it does not mean that all state changes for the given resource should use a complex operation. Simple state transitions (i.e. changes to a `status` field) should still utilize PUT/PATCH. It is completely appropriate to mix patterns using PUT/PATCH on a [Resource Collection](#resource-collection) + Complex Operation, as to minimize the number of operations.

###### Example Request (for mixed use of PUT)
~~~
PATCH /v1/payments/billing-agreements/I-0LN988D3JACS
[
    {
        "op": "replace",
        "path": "/",
        "value": {
            "description": "New Description",
            "shipping_address": {
                "line1": "2065 Hamilton Ave",
                "city": "San Jose",
                "state": "CA",
                "postal_code": "95125",
                "country_code": "US"
            }
        }
    }
]
~~~

Keep in mind if there is any need to see the history of these actions, a [Sub-resource Collection](#sub-resource-collection) is appropriate to show all of the prior executions of this action. In that case, the verb should be '[reified](http://en.wikipedia.org/wiki/Reification_(computer_science)'), or changed to a plural noun (e.g. 'execute' would become 'executions').

### Complex Operation - Composite

This type of complex operation creates/updates/deletes multiple resources in one operation. This serves as both a performance & usability optimization, as well as adding better atomicity when values in the request might affect multiple resources at the same time. 

Note in the sample below, the capture and the payment are both potentially affected by refund. A PUT or PATCH operation on the capture resource would have unintended side effects on the payment resource. To encapsulate both of these changes, the 'refund' action is used.

#### URI Template
    POST /{version}/{namespace}/{action}
#### Example Request
~~~
POST /v1/payments/captures/{capture-id}/refund
~~~
#### Example Response
~~~
{
    "id": "0P209507D6694645N",
    "create_time": "2013-05-06T22:11:51Z",
    "update_time": "2013-05-06T22:11:51Z",
    "state": "completed",
    "amount": {
        "total": "110.54",
        "currency": "USD"
    },
    "capture_id": "8F148933LY9388354",
    "parent_payment": "PAY-8PT597110X687430LKGECATA",
    "links": [
        {
            "href": "https://api.sandbox.paypal.com/v1/payments/refund/0P209507D6694645N",
            "rel": "self",
            "method": "GET"
        },
        {
            "href": "https://api.sandbox.paypal.com/v1/payments/payment/PAY-8PT597110X687430LKGECATA",
            "rel": "parent_payment",
            "method": "GET"
        },
        {
            "href": "https://api.sandbox.paypal.com/v1/payments/capture/8F148933LY9388354",
            "rel": "capture",
            "method": "GET"
        }
    ]
}
~~~

### Complex Operation - Transient

This type of complex operation doesn't maintain state for the client, and creates no resources. This is about as RPC as it gets; other alternatives should be considered first. 

This isn't usually utilized in sub-resources, as a sub-resource action would typically affect the parent resource.

HTTP status `200 OK` is always appropriate. Response body
contains calculated values, which could potentially change if run again.

As with all actions, [resource-oriented alternatives](#resource-oriented-alternative) should be considered first.

#### URI Template
    POST /{version}/{namespace}/{action}
#### Example Request
	POST /v1/risk/evaluate-payment
	{
		"code": "h43j5k6iop"
	}
#### Example Response
	200 OK
	{
		"status": "VALID"
	}

### Complex Operation - Search
When [Resource Collections](#resource-collection) are used, it's best to use query parameters on the collection to filter the set. However, there are some situations which demand a very complex search syntax, where query parameter filtering on a collection might present usability problems, or run up against theoretical query parameter length limitations.

In these situations, POST can be utilized with a request object to specify the search parameters. 

#### Paging
Assuming paging will be required with large response quantities, it's important to remember that the consumer will need to use POST on each subsequent page. As such, it's important to maintain paging in the query parameters (one of the rare exceptions where POST body + query parameters are utilized). 

Paging query parameters should follow the same conventions as in [Resource Collections](#paging).

This allows for hypermedia links to provide `next`, `previous`, `first`, `last` page rels with paging specified in the URL.

#### URI Template
	POST /{version}/{namespace}/{search-resource}
	
#### Example Request
	POST /v1/factory/widgets-search
	{
		"created_before":"1975-05-13",
		"status": "ACTIVE",
		"vendor": "Parts Inc."
	}
#### Example Response
	200 OK
	{
		"items": [
			<<lots of part objects here>>
		]
		"links": [
                {
                    "href": "https://api.sandbox.factory.io/v1/factory/widgets-search?page=2&page_size=10",
                    "rel": "next",
                    "method": "POST"
                },
				{
                    "href": "https://api.sandbox.factory.io/v1/factory/widgets-search?page=124&page_size=10",
                    "rel": "last",
                    "method": "POST"
                },
		]
	}

## Read-only resources
In situations where highly cacheable data is utilized to calculate values, or provide static reference, GET can be utilized instead of POST, as POST is not cacheable in HTTP. As POST is not cacheable, GET may be more appropriate.

GET operations should be idempotent, and should not be used to change resource state. POST should be used with [Complex Operations](#complex-operation).

### URI Template
	GET /{version}/{namespace}/{read-only-resource}
	
### Example Request
	GET /v1/location/geocode?address=77+N.+Washington+Street%2C+Boston%2C+MA%2C+02114
