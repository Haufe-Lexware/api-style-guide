# Haufe API style guide

###Purpose
---
Great RESTful APIs look like they were designed by a single team. This promotes API adoption, reduces friction, and enables clients to use them properly. To build APIs that meet this standard, and to answer many common questions encountered along the way of RESTful API development, the Haufe-Lexware CTO team has created this comprehensive set of guidelines. We have shared it with you to inspire additional discussion and refinement within and among your teams, and contribute our learnings and suggestions to the tech community at large.


###Usage
---
The API style guide MUST be used when you create an API, refactor or extend an API, no exeptions. Existing APIs might be subject to adapt if there is business value in it.

It is the responsibility of the person implementing the API to apply the API Styleguide to a specific API and project!
	
>	Please follow the guidelines, but don't follow blindly!  
>	You can break the rules by talking to us and providing justification.

The style guide is work in progress. We’d love your feedback – whether you agree, disagree, or have some additional practices and tips to add.

We encourage you to improve the style guide with modifications and extensions via the usual Fork/Pull Request dance. If your modification gets merged, it becomes part of the Styleguide. On the other hand if you don't bother, don't complain.

### API Design Review Process
---
We are still at the beginning of our journey to an API driven company.
   
There will be pros and cons how to handle and apply the style guide to real projects. There are many things to learn and to share with another. Feel free to reach out and ask and share on the `#apistrat` channel on our Haufe RocketChat.

Each new or refactored API MUST be reviewed through a API Design Review. Have a look at [API Design Review Process](api-design-review-process.md) for more information.

### Content
---

#### Most important chapters
Please read the first 6 chapters (in bold). These chapters describe the intention, mindset and process how we want to design an API.

Each section contains links to the chapters with more details.

#### Table of Content
The chapters are:

- **[Introduction](introduction.md)**
- **[General Guidlines](general-guidelines.md)**
- **[API Design Principles](api-design-principles.md)**
- **[API Design Process](api-design-process.md)**
- **[REST Principles](rest-principles.md)**
- [URI Components](uri-components.md)
- [HTTP Verbs](http-verbs.md)
- [Hypermedia and REST](hypermedia-and-rest.md) 
- [Resources](resources.md)
- [Relationships and Sub-Resources](relationships-and-sub-resources.md)
- [Collection Resources](collection-resources.md)
	- [Resource naming](collection-resources.md#resource-naming) 
	- [Get List of resources](collection-resources.md#get-list-of-resources)
	- [Read Single Resource](collection-resources.md#read-single-resources)
	- [Update Single Ressource](collection-resources.md#update-single-ressource)
	- [Update Partial Single Resource](collection-resources.md#update-partial-single-resource)
	- [Delete Single Resource](collection-resources.md#delete-single-resource)
	- [Create New Resource](collection-resources.md#create-new-resource)
	- [Create New Resource - Consumer Supplied Identifier](collection-resources.md#create-new-resource---consumer-supplied-identifier) 
- [Filtering, sorting, field selection and paging](filtering-sorting-field-selection-and-paging.md)
- [Hypermedia Response Format](response-format.md)
- [Type Formatting](type-formatting.md)
- [Message Schema and Postels Law](message-schema.md)
- [Security and Authentication](security-and-authentication.md)
- [Search](search.md)
- [Documentation](documentation.md)
- [Error handling](error-handling.md)
- [HTTP Status Codes](http-status-codes.md)
- [Caching](caching.md)
- [API Management](api-management.md)
- [FAQ](faq.md)
- [Further Reading](further-reading.md)

### Credits
---

For the creation of the style guide I took a lot of input from other authors and even copied whole passages.

I want to gratefully thank these authors and hope that I marked the relevant passages. My resources are listed under [Further Reading](further-reading.md). Special thanks goes to

[S.Stedman and G. Laforge from Paypal](https://github.com/paypal/api-standards/blob/master/api-style-guide.md)  
[Brian Mulloy](https://pages.apigee.com/rs/apigee/images/api-design-ebook-2012-03.pdf)  
[Geert Jansen](http://restful-api-design.readthedocs.org/en/latest/intro.html)   
[Vinay Sahni](http://www.vinaysahni.com/)  
[Michel Triana](http://micheltriana.com/2013/09/30/http-verbs-in-a-rest-web-api/)  
[Stefan Jauker](http://blog.mwaysolutions.com/author/stefan-jauker/)

Another great inspiration were [Zalando's Restful API Guidelines](http://zalando.github.io/restful-api-guidelines/).

