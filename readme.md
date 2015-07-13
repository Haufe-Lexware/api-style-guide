# Haufe API style guide

## Introduction

Purpose of this style guide is to gather a list of rules, best practices, resources and our way of creating REST APIs in Haufe Group.
The style guide addresses API Designers, mostly developers and architects, who want to design an API. Intention is to ease the design process by providing helpful rules to create a succesful API that your customers will love.
Please follow the guidelines but don't follow blindly! You can break the rules with justification. Please inform the CTO Office (mailto:_CTOLeads@haufe-lexware.com) in this cases.

The style guide focuses on REST APIs cause this is the preferred way to expose APIs to our services.

For the creation of the style guide I took a lot of input from other authors and even copied whole passages.

I want to gratefully thank these authors and hope that I marked the relevant passages.
My resources are listed under [Further Resources](further-resources.md). Special thanks goes to

[S.Stedman and G. Laforge from Paypal](https://github.com/paypal/api-standards/blob/master/api-style-guide.md)  
[Brian Mulloy](https://pages.apigee.com/rs/apigee/images/api-design-ebook-2012-03.pdf)  
[Geert Jansen](http://restful-api-design.readthedocs.org/en/latest/intro.html)   
[Vinay Sahni](http://www.vinaysahni.com/)  
[Michel Triana](http://micheltriana.com/2013/09/30/http-verbs-in-a-rest-web-api/)  
[Stefan Jauker](http://blog.mwaysolutions.com/author/stefan-jauker/)

The style guide is work in progress. We’d love your feedback – whether you agree, disagree, or have some additional practices and tips to add.

	Please contribute! 
 

The covered chapters are:

- [API design best practices](api-design-best-practices.md)
- [REST principles](rest-principles.md)
- [URI Components](uri-components.md)
- [HTTP Verbs](http-verbs.md)
- [Hypermedia and REST](hypermedia-and-rest.md) 
- [Resources](resources.md)
- [Relationships and Sub-Resources](relationships-and-sub-resources.md)
- [Collection Resources](collection-resources.md)
	- [Resource naming](collection-resources.md#resource naming) 
	- [Get List of resources](collection-resources.md#Get List of resources)
	- [Read Single Resource](collection-resources.md)
	- [Update Single Ressource](collection-resources.md)
	- [Update Partial Single Resource](collection-resources.md)
	- [Delete Single Resource](collection-resources.md)
	- [Create New Resource](collection-resources.md)
	- [Create New Resource - Consumer Supplied Identifier](collection-resources.md) 
- [Filtering, sorting, field selection and paging](filtering-sorting-field-selectiion-and-paging.md)
- [Response Format](response-format.md)
- [Type Formatting](type-formatting.md)
- [Security and Authentication](security-and-authentication.md)
- [Search](search.md)
- [Documentation](documentation.md)
- [Error handling](error-handling.md)
- [HTTP Status Codes](http-status-codes.md)
- [Caching](caching.md)
- [Further Resources](further-resources.md)

