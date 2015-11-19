##Caching

There are some reasons why you have to consider to use Caching

- improve Speed
- reduce latency
- reduce server load
- reduce network traffic 

Goal of caching is to never generate the same response twice.

> Tip: For security reasons, do not allow sensitive data to be cached.
Use server side mechanisms like setting `Cache-Control` to `no-store` in the response header.
Additionally you should use HTTPS.


#### Expires HTTP Header

The `Expires` HTTP header is a basic means of controlling caches; it tells all caches how long the associated representation is fresh for. 
After that time, caches will always check back with the origin server to see if a document is changed. 
`Expires` headers are supported by practically every cache.
Expires headers are especially good for making static images (like navigation bars and buttons) cacheable. Because they don’t change much, you can set extremely long expiry time on them, making your site appear much more responsive to your users. 

    GET /user_management/v1/users/BE14A7269802498F992813885546D058


    HTTP/1.1 200 OK
    ...
    Expires: Fri, 30 Oct 2015 14:19:41 GMT
    Last-Modified: Mon, 29 Jun 2015 02:28:12 GMT
    Content-Type: text/json; charset=utf-8
    Content-Length: ...
    { "id": "BE14A7269802498F992813885546D058", "name": "Mustermann" }

You can find more infos in:
[https://www.mnot.net/cache_docs/#CACHE-CONTROL](https://www.mnot.net/cache_docs/#EXPIRES) 

#### Cache-Control Header
Use `Cache-Control` header that indicates whether the data in the body of the response can be safely cached by the client or an intermediate server.
Use `max-age` to indicate after how many seconds the response should be considered out-of-date.
The following example shows an HTTP GET request and the corresponding response that includes a Cache-Control header:

    GET /user_management/v1/users/BE14A7269802498F992813885546D058


    HTTP/1.1 200 OK
    ...
    Cache-Control: max-age=600, private
    Content-Type: text/json; charset=utf-8
    Content-Length: ...
    { "id": "BE14A7269802498F992813885546D058", "name": "Mustermann" }

You can find a detailed explanation of the different `Cache-Control` opportunities in:
[https://www.mnot.net/cache_docs/#CACHE-CONTROL](https://www.mnot.net/cache_docs/#CACHE-CONTROL) 

#### ETag
You can use an ETag (Entity Tag) in the response header for entity data.
An ETag is an opaque string that indicates the version of a resource; each time a resource changes the Etag is also modified. 
This ETag should be cached as part of the data by the client application.
The ETag is primarily useful to save bandwidth.

    GET /user_management/v1/users/BE14A7269802498F992813885546D058

    HTTP/1.1 200 OK
    ...
    Cache-Control: max-age=600, private
    Content-Type: text/json; charset=utf-8
    ETag: "2147483648"
    Content-Length: ...
    { "id": "BE14A7269802498F992813885546D058", "name": "Mustermann" }

The client constructs a GET request containing the ETag for the currently cached version of the resource referenced 
in an If-None-Match HTTP header:

    GET /user_management/v1/users/BE14A7269802498F992813885546D058
    If-None-Match: "2147483648"

##### ETag matches
Return an HTTP response with an empty message body and a status code of 304 (Not Modified).

##### ETag does not match
The data has changed and the web API should return an HTTP response with the new data in the message body and a status code of 200 (OK).

### Further reading
Find below a list of great articles on the topic **Caching**

[https://www.mnot.net/cache_docs/](https://www.mnot.net/cache_docs/)  
[http://restcookbook.com/Basics/caching/](http://restcookbook.com/Basics/caching/)  
[http://odino.org/rest-better-http-cache/](http://odino.org/rest-better-http-cache/)  
[https://www.subbu.org/blog/2005/01/http-Caching](https://www.subbu.org/blog/2005/01/http-Caching)

Resources for implemantion
[https://github.com/mspnp/azure-guidance/blob/master/API-Implementation.md#considerations-for-optimizing-client-side-data-access](https://github.com/mspnp/azure-guidance/blob/master/API-Implementation.md#considerations-for-optimizing-client-side-data-access)


> :bulb: Please give us Feedback which Caching strategy you choose and your experieneces.
