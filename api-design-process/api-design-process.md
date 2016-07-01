## API Modelling and Design Process

The following API Modelling Process has been excerpted from chapter 4 and chapter 6 of [A Practical Approach to API Design](https://leanpub.com/restful-api-design) by D. Keith Casey Jr and James Higginbotham. (**This is just a brief summary for educational purpose - please consider buying the book to support their excellent work**)

### API Modelling Process
API modeling consists of 5 activities that help identify the requirements of your API design:1. Identify the participants, or actors, that will interact with your API2. Identify the activities that participants wish to achieve3. Separate the activities into steps that the participants will perform4. Create a list of API methods from the steps, grouped into common resource groups 5. Validate the API by using scenarios to test the completeness of the APIThe goal of each step is to explore the requirements of the API from a variety of different perspectives: those involved (the participants), what they want to do (the activities), and how they will complete an activity (the steps). The modeling process will be iterative, so it should be expected that each modeling activity may require that you revisit the previous step as previously hidden details are exposed.
### API Design Process
As you move from the modeling to the design phase, you will be faced with a variety of decisions. Some of these will resolve easily, while others will take time and deliberation. Just know that it is difficult to get your API design right the first time. This is why we encourage you to spend time designing and prototyping your API before you start implementing it. 

#### Building Your Resource Taxonomy
To get started building your taxonomy, make a list of the resources you have modeled already. These are often identified as the nouns in your system. Keep in mind that some resources may belong as a child collection of another resource.

#### How to Handle Resource Relationships and Composition
Resources often contain child resources or reference other resources. In these cases, the API design must ensure that one can interact with the top-level resource as well as nested and related resources.To better design our API, we need to understand the three types of resource relationships:
 Relationship | Description --------------|-------------
 Independent  | The resources exist stand alone without the other’s existence, but may reference each other        
 Dependent    | One resource cannot exist without the existence of the parent resource
 Associative  | The resources exist independently, however their relationship contains or requires additional properties to describe it
 How you design your API around related resources can be determined based on a few questions. The answers to these questions will determine if your resources are independent, dependent, or associative:
1. Can both resources exist without the other? Then we have an ***Independent*** resource2. Does one resource exist only when the other exists? Then we have a ***Dependent*** resource3. Does the relationship between resources require more state than just the links between them? Then we have an ***Associative*** resource
#### Defining Resource LifecyclesOnce we have identified our RESTful API resources, it is time to start mapping them to our HTTP verbs. During the API Modelling Process, we were able to capture most activities using a specific list of actions: “list”, “clear”, “view”, etc. Depending on the verbs you commonly use during modeling, you will likely find that certain onces can be mapped directly to REST patterns.#### Mapping Response Codes For Success and Failure
Please see [HTTP Status Codes](../http-status-codes/http-status-codes.md) for a more in depth discussion.#### Expanding Resources Through Hypermedia LinkingAs you move from resource identification, it is time to explore the links (relationships) each resource will provide. Resource links are just like links within a web page - they describe the options available for navigation. Links may be used for a variety of reasons:
1. To inform clients about the actions available given the resource’s current state2. Provide hints to actions that are authorized for the current user 
3. Allow clients to explore resource relationships

The following is a quick summary of Hypermedia Linking 

* ***Link Thyself***: Every resource should define a link to itself. While this seems to be redundant, or perhaps wasteful, there is a valid reason for returning a link to a returned resource: clients should never have to track the link that returned a resource separate from the resource representation itself.
* ***Resource State Linking***: Look for any constraints and state transitions that a resource may require. These constraints should be surfaced as links, offering insight into what actions are allowed and not allowed given the current state of a resource.	
* ***Relationship Linking***: Resources should include links to a parent resource, children, or related resources as identified during the previous taxonomy step.
* ***Additional Navigation Links***: Most commonly used for collection responses, where you may need to offer pagination links for next and previous pages, and for navigation to other areas of the API.

Please see [Hypermedia and Rest](../hypermedia-and-rest/hypermedia-and-rest.md) for a more in depth discussion of Hypermedia.

---

A good reference book for APIs and API Design is the afore mentioned [A Practical Approach to API Design](https://leanpub.com/restful-api-design) by D. Keith Casey Jr and James Higginbotham.