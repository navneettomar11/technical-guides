# What Is an API ?
First let's define what an API is. According to Wikipedia, an API(application programming interface) is: 
> "a set of subroutine definitions, communication protocols, and tools for building software. In general terms, it is a set of clearly defined methods of communication between various components."

An easy way to think about an API is to think of it as a contract of actions you can request for a particular service. APIs are in use today in a multitude of web applications, such as social media, banking software and much more. The standarized contract allows for external applications to interface with another.

For instances, let's say you're building an application that's going to integrate with Facebook. You would be able to use the Facebook Graph API to access data inside. Facebook, such as users, post, comments and more. The API simplifies the complexity of trying to use the data inside Facebook and provides an easy-to-use way for the developer to access that data.

# What is a Microservices?
Wikipedia defines a microservice as:
> a software development techinique - a variant of the servie-orient archiecture(SOA) architectural style that structures an application as a collection of loosely couples services. In a microservices archiecture, services are find-grained and the protocal are ligntweight.

But before we dig deeper into what microservces are and how they can be useful, let's take a quick lool into the monolith. Understanding how microservices differ from monoliths will give you a bettewr sense of the benefits of moving to a microservices architecture.

## The Precursor to Microservices: Monoliths
In the early days of software development (and continuing in many large enterprise environments today), there's the concept of a monolith. A monolith is a single application that holds a full collection of functionality, serving as one place to store everything. Architecturally, it look like this:
![Monoliths](assets/monolith.png)

All of the components of the application reside in one area, including the UI layer, the business logic layer, and the data access layer. Building applications in monoliths is an easy and natural process, and most projects start this way. But adding functionality to the codebase causes an increase in both the size and complexity of the monolith, and allowing a monolith to grow large comes with disadvantages over time.


# What is service-oriented architecture ?
Service-oriented architecture was largely created as a response to traditional, monolithic approaches to building applications. SOA breaks up the component required for applications into separate service modules that communicate with one another to meet specific business objectives. Each module is considerably smaller than a monolithic application, and can be deployed to serve different purposes in an enterprise. Additionally, SOA is delivered via the cloud and can include services for infrastructure, platforms, and applications.

SOA's two main roles are as a service provider and a service consumer. Its service provider layer includes the different services involved in SOA, while the consumer layer operates as the user interface.
SOA delivers four different type of services:
1. **Functional Services** are used for business operations.
2. **Enterprise services** implement the functionality
3. **Application services** are specific for developing and deploying apps.
4. **Infrastructure services** are for non-functional processes such as security and authentication.

Traditionally, SOA involves an enterprise service bus (ESB) as a means of coordinating and controlling.

# What is a microservices ?
Microservice architecture is generally considered an evolution of SOA as its services are more fine grained, anf function independently of each other. Therefore, if one of the services fail within an application, the app will continue to function since each service has a distinct purpose. The services in microservices communiate via application programming interfaces (APIs) and are organied around a particular business domains. Together, these services combine to make up complex applications.

Since each service is independent, a microservice architecture can scale better than other approaches used for application building and deployment. This characteristics also give microservice applications more fault tolerance thant other application development methods. Microservices are frequently built and deployed in the cloud; in many instances they operates in container.

# Microservice vs SOA: Identifying the differences
Many of the cheif characteristics of SOA and microservices are similar. Both involve a cloud or hybrid cloud environment for developing and running applications, are designed to combine multiple services necessary for creating and using applications, and each effectively breaks up large, complicated applications into smaller pieces that are more flexible to arrange and deploy. Because both microservices and SOA function in cloud settings, each can scale to meet the modern demands of big data size and speeds.

Nevertheless, there are many differences between SOA and microservices that determine the use cases each is suitable for:

||Microservices|SOA|
|**Architecture**|Designed to host services which can function independently| Designed to share resources across services|
|**Component Sharing**| Typically does not involve component sharing| Frequently involves component sharing|
|**Granularity**| Fine-grained services| Large, more modular services|
|**Data-Storage**| Each service can have an independent data storage| Involves sharing data storage between servicces|
|**Goverance**| Required collaboration between teams| Common goverance protocols across teams|
|**Size and Scope**| Better for smaller and web-based application| Better for large scale integrations|
|**Communication**| Communication through an API layer| Communicates through an ESB|
|**Coupling and cohesion**| Relies on bound context for copuling| Relies on sharing resources|
|**Remote Services**| Use Rest and JMS| Use protocols liks SOAP and AMQP|
|**Deployment**| Quick and easy deployment | Less flexibility in deployment|

