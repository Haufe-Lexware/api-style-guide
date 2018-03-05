## Correlation Id ##

It is extremely hard to analyse incorrect and faulty behaviour in a distributed system environment. Systems are calling other systems either synchronous or ansynchronous to fulfill a request. Each system logs to its own logging target. Manually analyzing the way of a request is a nightmare. To set up automated failure analytics is errorprone and brittle.
The [Correlation Pattern](http://www.enterpriseintegrationpatterns.com/patterns/messaging/CorrelationIdentifier.html), which depends on the use of Correlation ID is a well documented Enterprise Integration Pattern. Its target is to correlate requests over multiple systems and responses to another.

A Correlation ID is a unique identifier value that is attached to requests and messages that allow reference to a particular transaction or event chain. Attaching a Correlation ID to a request is arbitrary. You don’t have to use one. But if you are designing a distributed system that incorporates message queues and asynchronous processing, you will do well to include a Correlation ID in your messages. And even in synchronous chains it helps you to follow the call squence over different systems.

The Correlation Id is build by the client or as early as possible in the request chain. If a request contains no Correlation Id header it is up to each system to create and add it to the header.
It is the responsibility of each part/service/system in the chain to extract the Correlation Id from the request and use it in log entries. Calls to subsequent subsystems must contain the Correlation Id.



	{   
	   "Correlation-Id": <UUID>
	}

> :information_source: [Kong plugins](https://getkong.org/plugins/correlation-id/)

## Create Correlation Id with Wicked ##

Haufes [API Management Solution Wicked](http://wicked.haufe.io/) supports the creation of Correlation Id.
Read more [at](http://wickedhaufeio.readthedocs.io/en/stable/configuring-kong-plugins/).
