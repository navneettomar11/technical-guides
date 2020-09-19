# Saga Pattern
You have applied the `Database per service` pattern. Each service has its own database. Some business transactions, however, span multiple service so you need a mechanism to implement transactions that span services. For example, let's imagine that you are building an e-commerce store where customers have a credit limit. The application must ensure that a new order will not exceed the customer's credit limit. Since Orders and Customers are in different databases owned by different services the application cannot simply use a local ACID transaction.

## Problem
How to implement transactions that span services ?

## Forces
- 2PC is not an option

## Solution
Implement each business transaction that spans multiple services is a saga. A saga is a sequence of local transactions. Each local transactipn updates the database and publishes a message or event to trigger the next local transaction in the saga. If a local transaction fails because it voilates a business rules then saga executes a series of compensating transactions that undo the changes that were made by the preceding local transactions.

![From 2PC to Saga](assets/From_2PC_To_Saga.png)

There are two ways of coordianation sagas:
- **Chorenography** - each local transaction publishes domain event that trigger local transaction in other services.
- **Orchestration** - an orchestrator(object) tells the participants what local transactions to execute.

## Example: Choreography-based saga

![Ex- Choreography-baed](assets/Create_Order_Saga.png)

An e-comerce application that uses this approach would create an order using choreography-based saga that consists of the following steps
1. The `OrderService` receives the `POST /orders` request and creates an `Order` in a `PENDING` state.
2. It then emits an `Order Created` event.
3. The `Customer Service` event handler attempts to reverse credit.
4. It then emits an event indicating the outcome.
5. The `OrderService`'s event handler either approve or rejects the `Order`.

## Example: Orchestraton-based saga

![Ex-Orchestration-based saga](assets/Create_Order_Saga_Orchestration.png)

An e-comerce application that uses this approach would create an order using an orchestration-based saga that consists of the following steps:
1. The `Order Service` receives the `POST /orders` request and creates the `Create Order` saga orchestrator.
2. The saga orchestrator creates an `Order` in the `PENDING` state.
3. If then sends a `Reserve Credit` command to the `Customer Service`.
4. The `Customer Service` attempts to reverse credit.
5. It then sends back a reply message indicating the outcome.
6. The saga orchestrator either approves or rejects the `Order`.

## Resulting context
This pattern has the following benefits:
- It enables an application to maintain data consistency across multiple services without using distributed transaction.

This solution has the following drawbacks:
- The programming model is more complex. For example, a developer must design compensating transactions that explicitly undo changes made earilier in a saga.

There are also the following issues to address:
- In order to be reliable, a service must atomically update its database and publish a message/event. It cannot use the traditional mechanism of a distributed transaction that spans the database and the message broker. 



