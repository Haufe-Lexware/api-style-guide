##HTTP Verbs

Just like the HTTP status codes, there are many more verbs in the HTTP standard that, although they are in theory OK to use for your Web API, you can get by with just a few that helps to keep your API simple, and self-explanatory to its clients.

The full list of HTTP verbs from the spec can be found HERE, but we are going to focus on how to interpret the verbs and the actions that should be executed for each on in the context of a well-defined REST Web API. In the table there is also the result set that standard clients expect when they make requests with such VERBs. To better understand their proper use, we’ll use a sample resource endpoint called Users, where the (you guessed it) “Users” of our app are exposed via our Web API.

| Resource Sample|             | GET (Read)          | POST (Insert)          | PUT (update)                       | DELETE                 | Patch (partial update)    |
|----------------|-------------|---------------------|------------------------|------------------------------------|------------------------|---------------------------|
| api/users      | Action      | Get a list of users | Creates a user         | Batch update                       | Error                  | Batch update the users only with the attributes present in the request |
|                | Return      | List of users       | New user               | No payload, only HTTP Status Code  | Error HTTP Status Code |                           |
|                |             |                     |                        |                                    |                        |                           |
| api/users/123  | Action      | Get a single user   | Error                  | Updates the user                   | Deletes the user       | Partially updates the user only with the attributes present in the request | 
|                | Return      | Single user         | Error HTTP Status code | Updated user                       | No payload, only       |                           | 




And this is how you properly use the HTTP Verbs in a REST Web API.

> :information_source: This paragraph is mainly a copy from http://micheltriana.com/2013/09/30/http-verbs-in-a-rest-web-api/. Thanks to Michel Triana.
 

 
