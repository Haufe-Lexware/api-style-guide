## Message Schema and Postel's Law

One of the key best practices for creating robust services is embodied in the so called [Postels Law (or Robustness Principle)](https://en.wikipedia.org/wiki/Robustness_principle). It states that any protocol implementation should

> be conservative in what you send, be liberal in what you accept.

It focuses on the idea of gracefully adapting to communication that doesn't conform to the agreed specification/contract. It is important to notice though that it only talks about accepting potential missformed but otherwise sensical messages. It does not talk about if and how it should be processed (i.e. deliberatly ignoring unknown fields or values is one valid approach)

The ability to extend service definitions after the initial release without affecting already deployed clients is key to preserve agility and evolvability of those services. Well designed message schemas play a key part in either enabling or hindering a development team in evolving the service in the future. 

Unfortunatly message schemas tend to be used for two different purposes at the same time

1. For message-level security and 
2. As message-level contract. 

Designing a schema for (1) results in strongly typed message structures and designing it for (2) leads to the use of schema for auto-generate client code.

The result is a service client  which is too strict to be extensible without providing  the security we desire. If we were concerned about security we would never use generated code but always curate the client. If we were  concerned about extensibility we would indicate the presence of a field without wanting to be pinned down on a particular type for eternity.

The fallacy of  Web Services was that it promised  both in one convenient package and resulted in the creation of tightly coupled systems out of convenience.

If you use a schema to design the message contract, write the schema such that you enable extensibility and robustness. If you design it for message security, manually curate your client and not auto-generate it. 

API design is one of the places where a developer needs to be aware of the tradeoffs. We are inclined towards strongly typed schemas either for convenience (hey, I get the right code generated) or for security (an integer should never be a string) or a combination of both. It feels good to have our cake and eat it too. But what we loose is the ability to evolve the service contract after going live. 

This is why we should focus on creating a ‘good enough’ schema tailored for robust and evolvable message contracts, and not try to use it for security. Such a schema should have greatly relaxed constrains with deliberate extension points to later changes possible without impacting existing clients.

As API designers we should create a contract with our users (the client developer) as follows:

1.  If you send me a message that looks like {X} I will provide you with a response that has a defined structure.  This will always be true as long as I am available.
2.  I promise not to change the location of the data you need within this structure.  You can depend on that data existing in the location you see today as long as this API version exists.
3.  I promise not to change what these pieces of data mean.  
4.  You promise to ignore any data I provide that you did not expect.

It is this 4th point that is the important bit for client developers.  It means that client apps must parse/serialize/marshall data in a way that does not create a dependancy on messages with a static data structure.

It is easy to sell this type of extensibility to client developers when they work with weakly typed languages like JavaScript.  However, it can be more difficult when our applications are written in languages like C# or Java.  This may require more guidance and support.  For example, explicit instructions on best practices for parsing might be required: don't use explicit parsing, use these specific options for Saxon/JAXB/whatever...

The payoff for client application developers should be clear. Following your guidelines for extensilbity should create less work for them. If that isn't the case, then you will need to reconsider the cost-benefit for your particular application.  In the same vein, if the cost of changing client applications is very low, evolvability is not an important architectural quality.

Practical guidelines for robust and evolvable message schemas are

* No type definitions with restriction on character sets or regular expressions.
* Choose string over integer for elements like IDs that are likely to change its type.
* Avoid type definitions with enumerations.
* No upper bounds for lists.
* All elements are optional.

The following documentation might need to be added to the schema 

* To indicate which elements are usually part of a sensical message (formerly mandatory elements)
* Semantics of the elements (we need this anyway as no formal syntax can convey the meaning)
* To tell the client developers to prepare for additional elements or additional attributes
 
From an implementation point of view this might require the following

* Input validation on the server side for character sets
* Input validation for content that may open doors for SQL injections etc.
* A protection against XML DOS attacks
 
Auto-generating client and server side code might be permissable for as long as the following rules are met:

* There is no direct marshalling/unmarshalling of the actual business entity but of a data transfer object.
* It is assured that the code does not break when new elements or attributes are added to the schema.

Please note though that there might be benefits in allowing code-generation of some types like xsd:date for example.

To address security we need to consider both confidentiality of the message exchange and authentication of the message enpoints. The former will almost alwasy be provided by TLS and the latter through certificate validation and/or token-based authentication like OAuth2. More details can be found in [Security and Authentication](../security-and-authentication/security-and-authentication.md).
