## General Guidelines

### You should think of an API like a product. Indeed a Business API is a product.

Like a product it should follow these rules:  

- Easy to learn  
- Hard to misuse
- Easy to read and maintain it
- Audience oriented

### Design your API with a customer, or at least with a customer in mind and actively seek feedback.

Every API should be designed as if it were an external API for paying customers. Design your API with the API consumer in mind!

Each resource MUST make sense from the perspective of the API consumer. Beware of simply copy your internal model. Do not leak irrelevant implementation details out to your API.

**We believe that APIs are products**. We will provide these APIs to other business units and partners. The future is unpredictable. The conviction that an API is only internally or in one Business unit of use, can rapidly change. Therefore APIs should be designed like external APIs. Last but not least we learn to design better APIs.

### Do not implement API Management

We have a list of appropriate API management solutions we like to standardize on. If our existing solutions and blueprints do not fit, we work with you to find one which does. Refer to (API Management)[api-management.md] for details.

### Every API MUST be described using a formal API description language

While we recognize the existence of various languages (RAML, Blueprint) we standardize on Swagger as the most commonly used language. API documentation has to be generated from its Swagger definition to ensure inherent consistency. Swagger is used to provide access to the API via API management tooling. 

Reserve time to create documentation. Use [Swagger](http://swagger.io/) to document. Refer to [Documentation](../documentation/documentation.md) for details.

### API clients MUST use an API Key

Different SLA’s CAN be offered by the API provider. API clients are identified by API keys obtained from the API Portal. **No API access without a corresponding and valid API key**. (Exception for pubsub notification APIs which only contain the link to the resource but not the resource itself).
	
