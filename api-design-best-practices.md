## API design best practices

### You should think of an API like a product. Indeed a Business API is a product.

Like a product it should follow these rules:  

- Easy to learn  
- Hard to misuse
- Easy to read and maintain it
- Audience oriented

### Design your API with a customer, or at least with a customer in mind and Get Feedback.

Every API should be designed as if it were an external API for paying customers

We believe that APIs are products. We will provide these APIs to other business units and partners.
The future is unpredictable. The conviction that an API is only internally or in one Business unit of use, can rapidly change.
Therefore APIs should be designed like external APIs. Last but not least we learn to design better APIs.

### Do not implement API Management

We have a list of appropriate API management solutions both free and commercial we like to share.

### Every API MUST be described using a formal API description language

While we recognize the existence of various languages (RAML, Blueprint) we standardize on Swagger as the most commonly used language. 
API documentation has to be generated from its Swagger definition to ensure inherent consistency. Swagger is used to provide access to the API via API management tooling. 

Reserve time to create documentation. Use [Swagger](https://helloreverb.com/developers/swagger "Swagger") to document.

### API clients MUST use a self-service API portal to get access to the API via an API Key

Different SLA’s CAN be offered by the API provider. API clients are identified by API keys obtained from the portal. 
No API access without a corresponding and valid API key. (exception for pubsub notification APIs which only contain the link to the resource but not the resource itself).
	
> We are currently investigating different API management solution to support this requirement.
	


 
