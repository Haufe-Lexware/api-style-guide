##HTTP Status Codes
The following table lists the HTTP response status codes for the `GET` (retrieve), `POST` (create), `PUT` (modify), and `DELETE` operations.

<div><table><colgroup><col><col><col><col></colgroup><thead><tr><th>Response Code</th><th>HTTP Operation</th><th>Response Body Contents</th><th>Description</th></tr></thead><tbody><tr><td>200</td><td>GET, PUT, DELETE</td><td>Resource</td><td>No error, operation successful.</td></tr><tr><td>201 Created</td><td>POST</td><td>Resource that was created</td><td>Successful creation of a resource. The Location HTTP Header can provide a link to the newly created resource.</td></tr><tr><td>204 No Content</td><td>POST, PUT, DELETE</td><td>N/A</td><td>The request was processed successfully, but no response body is needed.</td></tr><tr><td>304 Not Modified</td><td>conditional GET</td><td>N/A</td><td>Resource has not been modified. <span style="line-height: 1.4285;">Used when HTTP caching headers are in play.</span></td></tr><tr><td>400 Bad Request</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>Malformed syntax or a bad query.</td></tr><tr><td>401 Unauthorized</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>Action requires user authentication.</td></tr><tr><td>403 Forbidden</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>

When authentication succeeded but authenticated user doesn't have access to the resource.

</td></tr><tr><td>404 Not Found</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>Resource not found.</td></tr><tr><td>405 Not Allowed</td><td>GET, POST, PUT, DELETE</td><td>Error Message</td><td>Method not allowed on resource.</td></tr><tr><td>406 Not Acceptable</td><td>GET</td><td>Error Message</td><td>Requested representation not available for the resource.</td></tr><tr><td>408 Request Timeout</td><td>GET, POST</td><td>Error Message</td><td>Request has timed out.</td></tr><tr><td>409 Resource Conflict</td><td>PUT, PUT, DELETE</td><td>Error Message</td><td>State of the resource doesn't permit request.</td></tr><tr><td>410 Gone</td><td>GET, PUT</td><td>Error Message</td><td>

Indicates that the resource at this end point is no longer available. Useful as a blanket response for old API versions

</td></tr><tr><td>411 Length Required</td><td>POST, PUT</td><td>Error Message</td><td>The server needs to know the size of the entity body and it should be specified in the Content Length header.</td></tr><tr><td>412 Precondition failed</td><td>GET</td><td>Error Message</td><td>Operation not completed because preconditions were not met.</td></tr><tr><td>415 Unsupported Type</td><td>POST, PUT</td><td>Error Message</td><td>Representation not supported for the resource.</td></tr><tr><td>422 Unprocessable Entity</td><td>POST, PUT</td><td>Error Message</td><td>

Used for validation errors

</td></tr><tr><td>500 Server Error</td><td>GET, POST, PUT</td><td>Error Message</td><td>Internal server error.</td></tr><tr><td>501 Not Implemented</td><td>POST, PUT, DELETE</td><td>Error Message</td><td>Requested HTTP operation not supported.</td></tr></tbody></table></div>
