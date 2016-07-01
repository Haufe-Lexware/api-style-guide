## URI Components
###Hostname

Make no assumption about the hostname. 
Assume that each API will have a different hostname. In particular do not assume a specific IP address. Services can be dynamically deployed or moved. 
DNS was invented to deal with that.

###Service namespace

In any URI, the first noun (which may be singular or plural, depending on the situation) should be considered a “Service namespace”. Service namespaces should reflect the customer's perspective on the (business) service boundary.

Do not use acronyms. Use small caps and underscore ‘_’ for ‘space’ (governs all of the url and messages). 

#####URI Template

	/{namespace}/

#####Example

	/user_management/

###Version

The URI should include /vN with the major version (N) as a prefix. No major.minor syntax. URL-based versioning is utilized for it's simplicity of use for API consumers, versus the more complex header-based approach.[^1]

[^1] See the following [brief overview of versioning methods](https://www.3scale.net/2016/06/api-versioning-methods-a-brief-reference/)

Version is a single number and only to be incremented on ‘backward’ compatibility breaking. Adding fields or deprecating fileds are **NOT** breaking changes. API Clients need to be able to **ignore** additional elements. API Providers need to be able to use meaningful defaults for new fields not expected by existing clients.  

Please stick to the following rule:

> In general do **NOT** increment the version of your API. Until you must. And it is really rare that you must. Only increment on breaking changes. Extending the API is not a breaking change. 

#####URI Template

	/{namespace}/v{version}/

#####Example

	/user_management/v1/

### Resource References

The URI references for resources should consistently use the same path components to refer to resources. Sub-namespace or sub-folders should be avoided, to maintain path consistency. This allows consumer developers to have a predictable experience in case they are building URIs in code.

#####URI Template

	/{namespace}/{version}/{resource}/{resource-id}/{sub-resource}/{sub-resource-id}
 

### baseurl

In the following sections of the guide the term **baseurl** is often used to indicate an absolute URL.
The **baseurl** is a replacement for the host, namespace and version.

#####URI Template

	baseurl = {scheme}://{host}/{namespace}/{version}

### Valid and invalid url

Your rest endpoints normally end with a noun like

    {host}/user_management/v1/users

There is no trailing slash. You CAN support a trailing slash. But it is not common. Do NOT support urls like {host}/user_management/v1/users/?queryparams. This is not valid syntax!


 
