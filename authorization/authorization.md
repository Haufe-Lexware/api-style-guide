## Authorization

After the user's identity has been established (authentication) we may have scenarios in which the user should be allowed or not to certain actions or information.

Since authorization can be connected to a varied number of information sources and specific to each project (licenses, rights, roles, identity etc.) it should not be delegated to a centralized solution.

## Access Control

There are multiple techniques that can be leveraged for access control. Depending on your scenario you may find you need one or maybe a combination of multiple methods.

A couple of examples (_in order of complexity_):

| Acronym 	| Name                               	| Description                                                                                      	|
|---------	|------------------------------------	|--------------------------------------------------------------------------------------------------	|
| ACL     	| Access Control List                	| Are you on the whitelist?                                                                        	|
| RBAC    	| Role Based Access Control          	| Are you an admin?                                                                                	|
| ABAC/PBAC	| Attribute/Policy Based Access Control	| Do you or the action or the resource have a certain value for a certain attribute ?            	|
| RABAC   	| Risk Adaptive Based Access Control 	| Contextually going to determine a risk score and decide if to allow or deny based on that score. 	|

Hybrid combinations of these approaches can be used.

For example you may have broad access control using RBAC (_i.e only admins have access to the admin API_) followed by ABAC/PBAC for more fine grained rules on the actions available (_i.e only admins from a specific department can grant other people the role of admin_).
 
## Field-level access control

In certain scenarios you might want to have different representations depending on who is making the call.

i.e.: When I ask for my User entity I should see all fields but when someone else asks for my user they should see just a minimum amount of data that is not private.

```json5
//representation for authorized user - GET /users/<myUserId>
{
    "firstName": "...",
    "lastName":"...",
    "age": "...",
    "sex":"...",
    "religion": "...",
    "politicalViews":"...",
    "avatarUrl": "...",
    "jobTitle":"...",
    "address": "...",
    "companyName":"..."
}
```
```json5
//representation for other users - GET /users/<otherUserId>
{
    "firstName": "...",
    "lastName":"...",
    "jobTitle": "...",
    "avatarUrl":"..."
}
```

A direct consequence of this is that you need to mark the potentially blocked fields as optional in the API documentation.

This in conjunction with the field selection approach via query parameters detailed in the [filtering, sorting, field selection and paging](../filtering-sorting-field-selection-and-paging/filtering-sorting-field-selection-and-paging.md#field-selection) section can lead to a more complex scenario where we must write authorization logic based on the fields requested.

If one of the fields requested cannot be returned due to authorization rules then a `403 Forbidden` should be returned.



