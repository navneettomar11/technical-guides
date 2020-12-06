# Understanding Transaction annotation in Spring
Spring provides support for both programmatic and declarative transactions.

## Programmatic Transaction
With Programmatic transactions, transaction management code needs to be explicitly written so as to commit when everything is successful and rolling back if any goes wrong. The transaction management code is tightly bound to the business logic in this case.

## Delcarative Transactions
Declarative transactions separates transaction management code from the business logic. Spring supports declarative transactions using transaction advice (using AOP) via XML configuration in the spring context or with **@Transactional** annotation.

### Implementation
To start using @Transactional annotation in a Spring based application, we need to first enable annotation in our spring application by adding the needed configuration into spring context file - 
```xml
<tx:annotation-driven transaction-manager="txManager"/>
```
Next is to define the transaction manager bean, with the same name as specified in the above transaction manager attribute value.

We are now ready to use `@Transactional` annotation either at the class or method level.
```java
@Transactional(value = "myTransactionManager", propagation = Propagation.REQUIRED, readOnly = true)
public void myMethod() {
    ...
}
```

## Understanding @Transactional annotation
At a high level, when a class declares @Transactional on itself or its members, Spring creates a proxy that implements the same interface(s) as the class you're annotating. In other words, Spring wraps the bean in the proxy and the bean itself has no knowledge of it. A proxy provides a way for Spring to inject behaviors before, after or around methods calls into the object being proxied.
 
Internally, its the same as using a transaction advice (using AOP), where proxy is created first and is invoked before/after the target bean's method.

The generated proxy object is supplied with a TransactionInterceptor, which is created by Spring. So, when the `@Transactional` method is called from client code, the TransactionInterceptor get invoked first from the proxy object, which begins the transaction and eventually invokes the method on the target bean. When the invocaton finishes, the TransactionInterceptor commits/rolls back the transaction accordingly.

A high level, Spring create proxies for all the classes annotated with @Transactional - either on the class or on any of the methods. The proxy allows the framework to inject transactional logic before and after the running method - mainly for starting and committing the transaction.

