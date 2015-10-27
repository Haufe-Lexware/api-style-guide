##Filtering, sorting, field selection and paging
###Filtering
Use a unique query parameter for all fields or a query language for filtering.

	GET /cars?color=red Returns a list of red cars
	GET /cars?seats<=2 Returns a list of cars with a maximum of 2 seats
 
###Time selection

start_time or {property_name}_after, end_time or {property_name}_before query parameters should be provided if time selection is needed.

###Sorting

Allow ascending and descending sorting over multiple fields.

	GET /cars?sort=-manufactorer,+model
 

This returns a list of cars sorted by descending manufacturers and ascending models.

The sort order for each sort field **MUST** be specified with one of the following prefixes:

- Plus (U+002B PLUS SIGN, "+") to request an ascending sort order.
- Minus (U+002D HYPHEN-MINUS, "-") to request a descending sort order.

###Field selection

Mobile clients display just a few attributes in a list. They don’t need all attributes of a resource. Give the API consumer the ability to choose returned fields. This will also reduce the network traffic and speed up the usage of the API.

	GET /cars?fields=manufacturer,model,id,color
 

###Paging

There are two popular ways how to support paging.

####limit and offset style

Use **limit** and **offset**. It is flexible for the user and common in API implementations (Facebook). The default should be limit=**20** and offset=**0**.

	GET /products?limit=25&offset=50
 

#####Response Fields
	{ 
	   "limit": 25,
	   "offset": 50,
	   "total_count": 1634
	} 
	 
|Name|Type|Description|
|---|---|---|
|offset|Integer|The offset used in the execution of the query.| 
|limit|Integer|The limit used in the execution of the query.| 
|total_count|Integer|The total number of records available.| 

####page style

Pages of results should be referred to consistently by the query parameters **page** and **page_size**, where **page_size** refers to the amount of results per request, and **page** refers to the requested page.Additionally, responses should include **total_count** and **total_pages** whenever possible, where **total_count** indicates the total records in the requested collection, and **total_pages** is the number of pages (interpolated from total_count/page_size).
The default should be page=**1** and page_size=**20**.

	GET /products?page=2&page_size=30
 

#####Response Fields

	{ 
	   "page": 2,
	   "page_size": 30,
	   "total_count": 1634,
	   "total_pages": 55
	} 

|Name|Type|Description|
|---|---|---|
|page|Integer|The page used in the execution of the query.|
|page_size|Integer|The page_size used in the execution of the query.|
|total_count|Integer|The total number of records available.| 
|total_pages|Integer|The total number of pages available| 

###Hypermedia links

Hypermedia links are high value in navigating paged resource collections, as page/page_size query parameters can be maintained while navigating pages of results.

Links should be provided with rels of `next`, `previous`, `first`, `last` wherever appropriate.

 

 
