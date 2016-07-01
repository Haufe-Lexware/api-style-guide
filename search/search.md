##Search

###Tips for search

###Simple or scoped search

A simple search could be modeled as a resourceful API by appending '?' after the path of the URL and adding query parameters.  

#####Example

	/users?given_name="Mustermann"
 

Please refer to the filtering section in [Filtering, sorting, field selection and paging](../filtering-sorting-field-selection-and-paging/filtering-sorting-field-selection-and-paging.md).

###Global search

A more complex search across multiple resources requires a different design.

This will sound familiar if you've read the topic about using verbs not nouns when results don't return a resource from the database - rather the result is some action or calculation.

If you want to do a global search across resources, we suggest you follow the Google model:

	/search?q=fluffy+fur
 

Here, **search** is the verb and **?q**  represents the query.

###Using POST 

Another way to implement a RESTful search is to consider the search itself to be a resource.  Then you can use the POST verb because you are creating a search.  You do not have to literally create something in a database in order to use a POST. 

#####Example

	Accept: application/json 
	Content-Type: application/json 
	POST http://example.com/user_management/v1/users/searches
	{   
	   "name": "Mustermann" 
	} 
 

You are creating a search from the user's standpoint.  The implementation details of this are irrelevant.  Some RESTful APIs may not even need persistence.  That is an implementation detail.

For long running searches it is a good solution to split the the search request into the POST request to create the search and a GET request to retrieve the search results.

Otherwise it is ok to deliver the search results in the response of the POST call. This solution is more convenient for the client. 

 

 

 
