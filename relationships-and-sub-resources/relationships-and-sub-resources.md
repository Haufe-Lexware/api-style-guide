##Relationships and Sub-Resources
A resource is the fundamental unit in RESTful API design. Resources model objects from the application data model.
Resources almost always have relationships to other resources.

Our Goal is to provide self describing APIs where links help the API consumer how to use the API.

###Hypermedia Support for relationships

A response ...

- MUST return a link element to **self**
- MUST return links to **sub-resource**
- SHOULD return links to **related objects**

#####Example Request

	GET /invoices/INV-567A89HG1
 

#####Example Response
HAL response Format

	{
	  "_links": {
	    "self": { "href": "{baseurl}/invoices/INV-567A89HG1"},
	    "customer": { "href": "{baseurl}/customer/CUST-12ATCVWV"},
	    "collection/lineitems": { "href": "{baseurl}/invoices/INV-567A89HG1/lineitems"},
	    "collection/payments": { "href": "{baseurl}/invoices/INV-567A89HG1/payments"},
	  }
	  "id": "INV-567A89HG1",
	  "number" : "627726",
	  "total": 20.30,
	  "date" : "2015-06-09 12:00:00",
	  "customer_id": "CUST-12ATCVWV",
	  "customer_name": "Max Mustermann"
	}

###Modeling Relationships

Relationships are often modeled by a **sub-resource**.
Use the following pattern for sub-resources.

	GET  /{resource}/{resource-id}/{sub-resource}
	GET  /{resource}/{resource-id}/{sub-resource}/{sub-resource-id}
	POST /resource}/{resource-id}/{sub-resource}

> :information_source: Limit the depth of your URIs to ony one sub-resource to keep the API clean and simple

###When to use sub-resources

####Existantially dependent 

In case of a 1:N relationship, where the child object is existentially dependent on the parent object use a sub-resource. “Existentially dependent” means that a child object cannot exist without its parent. 
An example of such a relationship are lineitems of an invoice.

	GET  /invoices/INV-567A89HG1/lineitems
	POST /invoices/INV-567A89HG1/lineitems
 

####Belongs to relationship

If you have a "Belongs to" relationship use a sub-resource.

Invoices belong to a customer. To get all invoices to a specific customer, or to create a new invoice for that customer, do a GET or a POST:

	GET  /customers/CUST-12ATCVWV/invoices
	POST /customers/CUST-12ATCVWV/invoices
 

Other samples are  "A dog belongs to an owner",  "a shirt belongs to a person".

####N:M relationship 

In case of a N:M relationship use a sub-resource. 
The sub-resource can be defined initially to support the lookup direction that you most often need to follow. 
If you need to query both sides of the relationship often, then two sub-resources can be defined, one on each linked object. 

A good example is a car pool. There are N drivers and M cars. 

	GET /cars/CAR-3767FSHS/drivers
	GET /drivers/DRI-ZU99983/cars
 

###Using a query parameter instead of sub-resources

Another valid option is to use query parameters instead of sub-resources in case of the Belongs to and N:M relationship.

	GET  /invoices?customer_id=CUST-12ATCVWV
	POST /invoices
	 
	GET /drivers?car_id=CAR-3767FSHS
	GET /cars?driver_id=DRI-ZU99983
 
It is important to be consistent how to design your API.
