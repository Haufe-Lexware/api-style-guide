## API Design Best Practices

### You should think of an API like a product. Indeed a Business API is a product.

Like a product it should follow these rules:  

- Easy to learn  
- Hard to misuse
- Easy to read and maintain it
- Audience oriented

### Design your API with a customer, or at least with a customer in mind and Get Feedback.

Every API should be designed as if it were an external API for paying customers.
Design your API with the API consumer in mind!

Each resource MUST make sense from the perspective of the API consumer. Beware of simply copy your internal model. Do not leak irrelevant implementation details out to your API.

We believe that APIs are products. We will provide these APIs to other business units and partners.
The future is unpredictable. The conviction that an API is only internally or in one Business unit of use, can rapidly change.
Therefore APIs should be designed like external APIs. Last but not least we learn to design better APIs.

### Do not implement API Management

We have a list of appropriate API management solutions we like to standardize on. If our existing solutions and blueprints do not fit, we work with you to find one which does.

### Every API MUST be described using a formal API description language

While we recognize the existence of various languages (RAML, Blueprint) we standardize on Swagger as the most commonly used language. 
API documentation has to be generated from its Swagger definition to ensure inherent consistency. Swagger is used to provide access to the API via API management tooling. 

Reserve time to create documentation. Use [Swagger](http://swagger.io/) to document. Refer to [Documentation](documentation.md) for details.

### API clients MUST use a self-service API portal to get access to the API via an API Key

Different SLA’s CAN be offered by the API provider. API clients are identified by API keys obtained from the portal. 
No API access without a corresponding and valid API key. (exception for pubsub notification APIs which only contain the link to the resource but not the resource itself).
	
> We are currently investigating different API management solution to support this requirement. A blueprint exists for Microsoft Azure and a self-contained Open Source solution based on Mashape Kong API Gateway.
	
### Follow the Robustness Principle

One of the key best practices for creating robust services is embodied in the so called [Postels Law (or Robustness Principle)](https://en.wikipedia.org/wiki/Robustness_principle). It states that any protocol implementation should

> be conservative in what you send, be liberal in what you accept.

The ability to extend service definitions after the initial release without affecting already deployed clients is key to preserve agility and evolvability of those services.
Unfortunatly message schemas tend to be used for two different purposes at the same time
Refer to [Message Schema and Postel's Law](message-schema.md)
 
