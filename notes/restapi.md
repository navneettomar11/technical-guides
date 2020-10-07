# Representational State Transfer
An API is an application programming interface. It is a set of rules that allow programs to talk to each other. The developer creates the API on the server and allows the client to talk to it.
**REST** determines how the API look like. It stands for `Representational State Transfer`. It is a set of rules that developer follow when they create their API. One of these states that you should be able to get a piece of data (called a resource) when you link to a specific URL.
Each URL is called a request while the data sent back to you is called a response.

## Idempotent operations
There are some HTTP methods e.g. GET which produces same response no matter how many times you use them e.g. sending multiple GET request to the same URI will result in same response without any side-effect hence it is known as idempotent.
On the other hand, the POST is not idempotent because if you send multiple POST request, it will result in multiple resource creation on the server, but again, PUT is idempotent if you are using it to update the resource.

# Spring RestTemplate
The RestTemplate clas is an implementation of Template method pattern in Spring framework. Similar to other popular template classes e.g. JdbcTemplate or JmsTemplate, it also simplifies the interaction with Restful Web Services on the client side. 