# Context
You are developing a large, complex application and want to use the microservice architecture. The microservice architecture structures an application as a set of loosely coupled services. The goal of the microservice architecture is to accelerate software development by enabling continous devlivery/deployment.

![Success Traingle](assets/successtriangle.png)

The microservice architecture does this in two ways:
1. Simplifies testing and enables components to deployed independently.
2. Structure the engineering organization as collection of small (6-10 members), autonomous teams, each of which is responsible for one or more services.

These benefits are not automatically guaranteed. Instead, they can only be achieved by the careful functional decomposition of the application into services.

A service must be small enough to be developed by a small team and to be easily tested. A useful guideline from object-oriented design (OOD) is the Single Responsibility Principle(SRP). The SRP defines a responsibility of a class as a reason to change and states that a class should only have one reason to change. It make sense to apply the SRP to service design as well and design services that are cohesive and implement a small set of strongly related functions.

The application also be decomposed in a way so that most new and changed requirements only affect a single service. That is because changes that affect multiple, services requires coordination across multiple teams, which slow down development. Another useful principle from OOD is the `Common Closure Principle` (CCP), which states that classes that change for the same reason should be in the same package. Perhaps for instance, two classes implement different aspects of the same business rule. The goal is that when that business rule changes developer, only need to change code in a smaller number - ideally only one - of packages. This kinf of thinking makes sense when designing services since it will help ensure that each change should impact only one service.

# Problem
How to decompose an application into services?

## Force
- The architecture must be stable.
- Services must be cohesive. A service should implement a small set of strongly related functions.
- Services must conform to common closure principle - thing that change together should be packaged together - to ensure that each change affect only on service.
- Services must be loosely coupled - each service as an API that encapsulates its implementation. The implementation can be change without affecting clients.
- A service should be testable.
- Each service be small enough to be developed by a "two pizza" team i.e. a team of 6-10 people.
- Each team that own one or more services must be autonomous. A team must be able to develop and deploy their services with minimal collaboration with other teams.

# Decompose by business capability 

Define services corresponding to business capabilities. A business capabilities is a concept from business archiecture modeling. It is something that a business does in order to generate value. A business capability often corresponds to a business object e.g. 
- _Order Management_ is responsible for orders.
- _Customer Management_ is responsible for customers.

Business Capabilities are often organized into multi-level hierarchy. For example, an enterprise application might have top-level categories such as Product/Service development, Product delivery, Demand generation, etc.

## Examples
The business capabilities of an online store include:
- Product catalog management
- Inventory management
- Order management
- Delivery management
- ...

The corresponding microservice architecture would have services corresponding to each of these capabilities.

![Decompose by Business Capabiities](assets/decompose-by-business-capability.png)

## Resulting Context

This pattern has the following benefits:
- Stable architecture since the business capabilities are relatively stable.
- Development teams are cross-functional, autonomous and organized around delivering business value rather than techinical features.
- Services are cohesive and loosely coupled.

## Issues
There are the following issues to address:
- **How to identify business capabilities?** Identifying business capabilities and hence services requires an understanding of the business. An organization's identified using an iterative process. Good starting points for identifying business capabilities are:
    - organization structure - different groups with an organization might correspond to business capbabilities or business capability groups.
    - high-level domain model - business capabilities often correspond to domain objects.

# Decompose by subdomain
Define service corresponding to Domain-Driven Design (DDD) subdomains. DDD refers to the application's problem space - the business - as the domain is consist of multiple subdomains. Each subdomain corresponds to a different part of the business.

DDD is an approach for building complex software applications centered on the development of an object oriented, domain model. A domain model captures knowledge about domain in a form that can be used to solve problem within that domain. It defines the vocabulary used by team - what DDD calls the "Ubitquitous language". The domain model is closely mirrored in the design and implementation of the application. DDD has two concepts that are incredibly useful when applying the microservice archiecture: subdomains and bounded contexts.

Subdomains can be classified as follows:
- Core - key differentiator for the business and most valuable part of the application.
- Supporting - related to what the business does but not a differentiator. These can be implemented in-house or outsourced.
- Generic - not specific to the business and are ideally implemented using the off the shell software.

## Examples
The subdomains of an online store include:
- Product catalog
- Inventory management
- Order management
- Delivery management
- ...

The corresponding microservice architecture would have services corresponding to each of these subdomains.

![Decompose by subdomain](assets/decompose-by-subdomain.png)

## Resulting Context
This pattern has the following benefits:
- Stable archiecture since the subdomains and relatively stable.
- Developement teams are cross-functional, autonomous, and organized around delivering business value rather than technical features.
- Services are cohesive and loosely coupled.

## Issues
There are the following issues to address:
- **How to identify the subdomains ? Identifying subdomains and hence services requires an understanding of the business. Like business capabilities, subdomains are identified by analyzing the business and its organizational structure and identifying the different area of expertise. Subdomains are best identified using iterative process. Good starting points for identifying subdomains are:
    - organization structure - different groups withing an organization might correspond to subdomains
    - high-level domain model - subdomains often have a key domain object.
