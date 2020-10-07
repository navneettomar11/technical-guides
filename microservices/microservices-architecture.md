# Microservices Architecture
You are developing a server-side enterprise application. It must support a variety of different clients including desktop browsers, mobile browsers and native mobile applications. The application might also expose an API for 3rd parties to consume. It might also integrate with other applications via either web services or a message broker. The application handles request (HTTP requests and messages) by executing business logic; accessing a database; exchanging messages with other systems; and returning a HTML/JSON/XML response. There are logical components corresponding to different functional areas of the application.

# Problem
What's the application's deployment archiecture?

# Forces
- There is a team of developers working on the application
- New team members must quickly become productive.
- The application must be easy to understand and modify.
- You want to pratice continous deployment of the application.
- You must run multiple instances of the application on multiple machines in order to satisfy scalability and availability requirements. 
- You want to take advantage of emerging technologies(framework, programming languages, etc).

# Solution
Define an architecture that structures the application as a set of loosely coupled, collaborating services. This approach corresponds to the Y-axis of the Scale cube. Each service is 
- Highly maintainable and testable - enables rapid and frequent developments and deployment.
- Loosely coupled with other services - enables a team to work independently the majority of time on their service(s) without being impacted by changes to other services and without affecting other services.
- Idenpendently deployable - enables to deploy their services without having to coordinate with other teams.
- Capable of being developed by small team - essential for high productivity by avoiding the high communication head of large teams.

Services communicate using either synchronous protocols such as HTTP/REST or asynchronous protocols such as AMQP. Services can be deployed independently of one another. Each service has it own database in order to be decoupled from other services. Data consistency between services is maintained using the `Sage Pattern`.

# Example
## Fictitious e-commerce application
Let's image that you are building an e-commerce application that takes order from customers, verifies inventory and available credit, and ships them. The appliction consists of several components including the StroeFrontUI, which implements the user interface, along with some backend services for checking credit, maintaing inventory and shipping orders. The applications consists of a set of services.

![Microservice Architecture](assets/Microservice_Architecture.png)

# Resulting Context

## Benefits
This solution has a number of benefits:
- Enables the continuous delivery and deployment of large complex applications.
    - Improved maintainability - each service is relatively small and so is easier to understand and change.
    - Better testability - services are smaller and faster to test
    - Better deployability - services can be deployed independently
    - It enables you to organize the development effort around multiple, autonomous teams. Each (so called two pizza) team own and is responsible for one or more services. Each team can develop, test, deploy and scale their services independently of all of the other teams.
- Each microservices is relatively small:
    - Easier for a developer to understand
    - The IDE is faster making developers more productive
    - The application starts faster, which makes developers more productive and speeds up deployments.
- Improved fault isolation. For example, if there is a memory leak in one service then only that service will be affected. The other services will continue to handle requests. In comparison, one misbehaving component of a monolithic architecture can bring down the entire system.
- Eliminates any long-term commitment to a technology stack. When developing a new service you can pick a new technology stack. Similarly, when making major changes to an existing services you can rewrite it using a new technology stack.

## Drawbacks
This solution has a number of drawbacks:
- Developers must deal with the additional complexity of creating distributed system:
    - Developers must implement the inter-service communication mechanism and deal partial failure.
    - Implementing request than span multiple services is more diffcult.
    - Testing the interactions between services is more diffcult.
    - Implementing requests that span multiple services requires careful coordination between the teams.
    - Developer tools/IDEs are oriented on building monolithic applications and don't provide explicit support for developing distributed applications.
- Deployment complexity. In production, there is also the operational complexity of deploying and managing a system comprised of many different services.
- Increased memory consumption. The microservice architecture replaces N monolithic application instances with NxM service instances. If each service run in its own JVM (or equivalent) which is usually necessary to isolate the instances, then there is overhead of M times as many JVM runtimes. Moreover, each service run on its own VM (e.g EC2 instance), as is the case at Netflix, the overhead is even higher.

# Issues
There are many issues that you must address.

## When to used the microservice architecture ?
One challenge with using this approach is deciding when it makes sense to use it. When developing the first version of an application, you often do not have the problem that this approach solves. Moreover, using an elaborate, distributed architecture will slow down development. This can be a major problem for startups whose biggest challenge is often how to rapidly evolve the business model and accompanying application. Using Y-axis splits might make it much more diffcult to iterate rapidly. Later on, however, when the challenge is how to scale and you need to use functional decompostion, the tangled dependencies might make it diffcult to decompose you monolithic application into a set of services.

## How to decompose the application into services ?
Another challenge is deciding how to partition the system into microservices. This is very much an art, but there are a number of strategies that can help:
- Decompose by business capability and define services corresponding to business capabilities.
- Decompose by domain-driven design subdomain.
- Decompose by verb or use case and define services that are responsible for particular actions e.g. a `Shipping Service` that's responsible for shipping complete orders.
- Decompose by nouns or resources by defining a service that is responsible for all operation on entities/resource of a given type. e.g. an `Account Service` that is responsible for managing user accounts.

Ideally, each service should have only a small set of responsibilities. (Uncle) Bob Martin talks about desinging classes using `Single Responsibility Principle (SRP)`. The SRP defines a responsibilty of a class as a reason to change, and states that a class should only have one reason to change. It make sense to apply the SRP to service design as well.

Another analogy that helps with service design is the design of Unix utitlities. Unix provides a large number of utilities such as grep, cat and find. Each utility does exactly one thing, often exceptionally well, and can be combined with other utilities using a shell script to perform complex tasks.

## How to maintain data consistency ? 
In order to ensure loose coupling, each service has it own database. Maintaining data consistency between services is a challenge because 2 phase commit/distributed transactions is not an option for many applications. An application must instead use the `Saga pattern`. A service publishes an event when it data changes. Other services consume that event and update their data. There are several way of reliably updating data and publish events including Event Sourcing and Transaction Log Trailing.

## How to implement queries ?
Another change is implementing queries that need to retrieve data owned by multiple services.
- The `API Composition` and `Command Query Responsibility Segregation (CORS)` patterns.

# Core Concepts in Microservices

## Cohesion
Cohesion refers to the degree of focus that a particular software component has. A multi-purpose mobile phone with camera, radio, torch, etc. for example has low cohesion compared to a dedicated DLSR that does one thing better.

> Wikipedia -> Cohesion is software engineering is the degree to which the elements of a certain module belong together. In one sense, it is a measure of the strength of relationship between methods and data of a class and some unifying purpose or concept served by that class. In another sense, it is measure of the strength of relationship between the class's methods and data themselves.

Let us suppose we have a monolithic applicaton for a a fictitious e-shop, that does order management, inventory managmeent, user mangement, marketing, delivery management etc. This monolithic software has very low cohesion compared to a microsevices based architecture where each microservice is responsible for a particular business functionality, for example - User management microservice, Inventory management microservice, Order Management microservice and so on.

### Benefits of High Cohesion
1. Reduced modular complexity, because each module does one thing at a time.
2. Increased system maintainability, because logical changes in the domain affect fewer modules and because changes in one module require fewer changes in other modules.
3. Increased modules reusability, because application developers will find the component they need more easily among the cohesive set of operations provided by the module.
4. Easy understandability and testability.

Microservice in general should have a high cohesion i.e each microservice should do one thing and do it well.

## Coupling
Coupling refers to the degree of dependency that exists between two software components.

A very good day to day example of coupling is mobile handset that has battery sealed into the handset. Design in this case is tightly coupled beacuse battery or motherboard can not replaced from each other without affecting each other.

In Object Oriented Design we always look for low coupling among various components so as to achieve flexibility and good maintainability. Change in one components shall not affect other components of the system.

> High cohesion is almost always related to low coupling.

