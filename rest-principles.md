### Support Hypermedia

All our REST APIs MUST support hypermedia. Refer to [Hypermedia and REST](hypermedia-and-rest.md) for a detailed explanation.

### Use RESTful URLs and actions

Separate your API in logical [resources](resources.md).

There should be two base URIs per resource. The first URL is for a collection

	/products

The second is for a specific element in the collection.

	/products/1234
 
Use HTTP verbs to operate on resources.

Use sub-resources for relations.

### Nouns are good - Verbs are bad

**Use Nouns not Verbs** when defining the API. The best is to have them in **plural form** because in most of the cases it returns a list or a couple of elements, not just one.

### Use HTTP verbs to operate on the collections and elements

Use the HTTP verbs POST, GET, PUT, and DELETE. 
Think of them as mapping to the acronym, CRUD (Create-Read-Update-Delete).

### Plural nouns and concrete names

Given that the first thing most people probably do with a RESTful API is a GET, we think it reads more easily and is more intuitive to use plural nouns.

Think about an API that exposes employees, customers and suppliers. An API that models everything at the highest level of abstraction and uses /parties is not intuitive. Developers do not understand how to use the API. It is more compelling and useful to see the resources listed as employees, customers, and suppliers.

The level of abstraction depends on your scenario. You also want to expose a manageable number of resources. Aim for concrete naming and to keep the number of resources between 12 and 24.

### Use sub-resources for Relations

Use sub-resources for related resources. Limit the nesting to one sub-resource.

### Resource naming

Should you use hyphens, underscores, or camelCase to delimit words in the URIs?

There are many, many different articles and opinions about that. Finally we decided to go with underscores.

Use underscores. Do not use acronyms. Use small caps.

Look at these links for our reasoning. [https://whathecode.wordpress.com/2011/02/10/camelcase-vs-underscores-scientific-showdown/](https://whathecode.wordpress.com/2011/02/10/camelcase-vs-underscores-scientific-showdown/).

### Be consistent

Being consistent allows developers to predict and guess the method calls as they learn to work with your API.

### Things to avoid
- Avoid query args on an non-query/non-search resource. e.g. prefer /conversations/conversation-12 over /conversations/conversation.php?conversation_id=12
- Do not use mixed or upper-case in URIs.
- Avoid extensions (avoid .en or .fr; avoid .html or .htm or .php or .jsp; avoid .xml or .json).
- Do not use characters that require url encoding in URIs (e.g. spaces).
