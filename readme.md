# Haufe API style guide

## Introduction

Purpose of this style guide is to gather a list of rules, best practices, resources and our way of creating REST APIs in **Haufe-Lexware**.
The style guide addresses API Designers, mostly developers and architects, who want to design an API. Intention is to ease the design process by providing helpful rules to create a succesful API that your customers will love.  
The style guide focuses on **REST APIs** cause this is the preferred way to expose APIs to our services.

###How to use the guide
The API style guide MUST be used when you create an API, refactor or extend an API.
Existing APIs might be subject to adapt if there is business value in it.

It is the responsibility of the API designer to apply the API style guide to a specific API and project!
	
>	Please follow the guidelines, but don't follow blindly!  
>	You can break the rules with justification.

The style guide is work in progress. We’d love your feedback – whether you agree, disagree, or have some additional practices and tips to add.

>	We encourage you to improve the style guide with modifications and extensions.
>	Inform the CTO Office (mailto:_CTOLeads@haufe-lexware.com) in this cases.
>
>	Please contribute! 

### API Design Review Process
We are still at the beginning of our journey to an API driven company.   
There will be pros and cons how to handle and apply the style guide to real projects.
There are many things to learn and to share with another.

To support the mutual experience exchange we decided to establish an **API Design Review Process**.
Each new or refactored API MUST run through this process.
Have a look at [API Design Review Process](api-design-review-process.md) for more information.

### Content

####Most important chapters
Please start with the chapters [API Design Best Practices](api-design-best-practices.md), [API Design Process](api-design-process.md) and [REST Principles](rest-principles.md). These chapters describe the intention, mindset and process how we want to design an API.
Each section contains links to the chapters with more details.

The covered chapters are:

- [API Design Best Practices](api-design-best-practices.md)
- [API Design Process](api-design-process.md)
- [REST Principles](rest-principles.md)
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
- [Further Resources](further-resources.md)

### Credits

For the creation of the style guide I took a lot of input from other authors and even copied whole passages.

I want to gratefully thank these authors and hope that I marked the relevant passages.
My resources are listed under [Further Resources](further-resources.md). Special thanks goes to

[S.Stedman and G. Laforge from Paypal](https://github.com/paypal/api-standards/blob/master/api-style-guide.md)  
[Brian Mulloy](https://pages.apigee.com/rs/apigee/images/api-design-ebook-2012-03.pdf)  
[Geert Jansen](http://restful-api-design.readthedocs.org/en/latest/intro.html)   
[Vinay Sahni](http://www.vinaysahni.com/)  
[Michel Triana](http://micheltriana.com/2013/09/30/http-verbs-in-a-rest-web-api/)  
[Stefan Jauker](http://blog.mwaysolutions.com/author/stefan-jauker/)


