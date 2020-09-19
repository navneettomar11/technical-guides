# Data Management
Let's imagine your are developing an online store application using the `Microservice architecture pattern`. Most services to persist data in some kind of databases. For example, the `Order Service` stores information about orders and the `Customer Service` stores information about customers.

![Customers and Orders Databases](assets/customersandorders.png)

## Problem
What's the database architecture in a microservices application?

## Forces
- Services must be loosely coupled so that they can be developed, deployed and scales independently.
- Some business transactions must enforce invariants that span multiples services. For example, the `Place Order` use case must verify that a new Order will not exceed the customer's credit limit. Other business transactions, must update data owned by multiple services.
- Some business transactions need to query data that is owned by multiple services. For example, the `View Available Credit` use must query the Customer to find the `creditLimit` and Orders to calculate the total amount of the open orders.
- Some queries must join data that is owned by multiple services. For example, finding customers in a particular region and their recent orders require a join between customers and orders.
- Databases must sometimes be replicated and shared in order to scale.
- Different services have different storage requirement. For some services, a relational database is the best choice. Other services might need a NoSQL database such as MongoDB, which is good at storing complex, unstructured data, or Neo4J which is designed to efficiently store and query graph data.

## Solution (Database per service)
Keep each microservice's persistent data private to that services and accessible only via its API. A service's transaction only invoice its database. 

The following diagrams shows the structure of this pattern.

![Database per servies](assets/databaseperservice.png)

The service's database is effectively part of the implementation of that service. It cannot be accessed directly by other services.

There are a few different ways to keep a service's persistent data private. You do not need to provision a database server for each service. For example, if you are using a relational database then the options are:
 - **Private-tables-per-service** - each service owns a set of tables that must only be accessed by that service.
 - **Schema-per-service**- each service has a database schema that's private to that service.
 - **Database-server-per-service** - each service has it's own database server.

 Private-tables-per-service and schema-per-service have the lowest overhead. Using a schema per service is appealing since it makes ownership cleaner. Some high throughput services might need their own database server.

 It is a good idea to create barriers that enforce this modularity. You could, for example, assign a different database user id to each service and use a database access control mechanism such as grants. Without some kind of barriers to enforce encapsulation, developers will always be tempted to bypass a service's API and access it's data directly.

## Resulting Context(Database per service)
Using a database per service has the following benefits:
- Helps ensure that the service are loosely coupled. Changes to one service database does not impact any other services.
- Each service can use the type of database that is best suited to its needs. For example, a service that does text searches could use ElasticSearch. A service that manipulates a social graph could use Neo4j.

Using a database per  service has the following drawbacks:
- Implementing business transactions that span multiple services is not straightforward. Distributed transactions are best avoided because of the CAP theorem. Moreover, many modern (NoSQL) database don't support them.
- Implementing queries that join data that is now in multiple database is challenging.
- Complexity of managing multiple SQL and NoSQL database.

There are various patterns/solutions for implementing trasactions and queries that span services.
- Implementing transaction that span services - use the `Saga pattern`.
- Implementing queries that span services.
    - `API Composition`: the application perform the join rather than database. For example, a service (or the API gateway) could retrieve a customer and their orders by first retrieving the customer from the customer service and then querying the order service to return the customer's most recent.
    - `Command Query Reponsibility Segregation (CORS)` - maintain one or more materialized views that contain data from multiple services. The views are kept by services that subscribe to events that each services publishes when it update its data. For example, the online store could implement a query that finds customers in a particular region and their recent orders by maintaining a view that joins customers and orders. The view is updated by service that subscribes to customers and order events.
    

## Solution(Shared Database)
Use a (single) database that is shared by multiple services. Each services freely accesses data owned by other services using local ACID transactions.

## Resulting Context(Shared Database)
The benefits of this pattern are:
- A developer use familiar and straightforward ACID transaction to enforce data consistency.
- A single database is simpler to operate.

The drawbacks of this pattern are -
- Development time coulping - a developer working on , for example, the OrderService will need to coordinate schema changes with the developers of other services that access the same tables. This coupling and additional coordination will slow down development.
- Runtime coupling - because all services access the same database they can potentially interface with one another. For example, if long running CustomerService transaction holds a lock on the ORDER table then the OrderService will be blocked.
- Single database might not satisfy the data storage and access requirement of all services.
