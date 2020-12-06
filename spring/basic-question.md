# What is Loose Coupling?
Loose coupling is an approach to interconnecting the components in a system or network so that those components, also called elements, depend on each other to the least extend praticable.

Loose coupling enables isolation. Components to be developed independently of one another, giving you much more freedom over when and what you deploy. Cross-functional delivery teams are able to get their work done without having to manage any dependencies on other teams.

Loose coupling helps provide better: 
* **Usability**: consistency in the user experience is improved since the same services are provided by a loosely coupled system even when it's replaced by alternative implementations.
* **Scalability**: easier to add components (horizontally scale).
* **Interoperbility**: Components in a loosely coupled syatem are less constrained to the same platform, language, operating system, or build environment.
* **Reusability**: Components which are mode smaller in a loosely coupled system can more likely be re-used for other purposes.

When an object gets the object to be used from the outside, then it is a loose coupling situation. As the main object is merely using the object, this object can be changed from the outside world easily marked it as loosely coupled objects.


# What is Dependency Injection?
In software engineering, dependency injection is a technique whereby one object supplies as the dependencies of another object. A dependency is an object that can be used (a service).

When class A use some functionality of class B, then this said that class A has a dependency of class B. 

In Java, before we can use methods of other classes, we first need to create the object of the class.

**So, transferring the task of creating the object to someone else and directly using the dependency is called dependency injection.**

Let's say we have a car class whict contains various objects such as wheels, engine, etc.

Here the car class is responsible for creating all the dependency objects. Now, what if we decide to ditch **MRFWheels** in the future and want to use **Apollo** Wheels?

We will need to recreate the car object with a new Apollo dependency. But when using dependency injection(DI), we can change the Wheels at runtime (because dependencies can be injected at runtime rather than at compile time).

You can thing of DI as the middleman in our code who does all the work of  creating the preferred wheels object and providing it to the Car class.

It makes our Car class independent from creating the objects of Wheels, Battery, etc.

Dependency Injection in Spring can be done through constructors, setter or fields.

Dependency injection (DI) is the concept in which objects get other required objects from outside.

# What is IOC (Inversion of Control)?
Inversion of Control(IoC) is a design principle. As the name suggests, it used to invert different kinds different kinds of control in object-oriented design to achieve loose coupling. Here, controls refers to any additional responsibilities, a class has, other than it main responsibility. This include control over the flow of an application, and control over the flow of an object creation or dependent object creation and binding.

IoC is all about inverting the control. To explain this in layman's term, suppose you drive a car to your work place. This mean you control the car. The IoC principle suggest to invert the control meaning that instead of driving the car yourself, you hire a cab where another person will drive the car. Thus, this called inversion of the control - from you to the cab driver. You don't have to drive a car yourself and you let the driver do the driving so that you can focus on your main work.

The advantage of this architecture are:
* decoupling the execution of a task from its implementation.
* making it easier to switch between different implementation.
* great modularity of a program.
* greater ease in testing a program by isolation component or mocking its dependencies and allowing component to communicate through contracts.

Inversion of Control can be achieved through various mechansims such as Strategy design pattern. Service Locator patern, Factory pattern and Dependency Injection(DI).

In the Spring framework, the IoC container is represented by the interface ApplicationContext. The Spring contaner is responsible for instantiating and assembling objects known as bean as well as managing their lifecycle.

# What is Auto Wiring?
Two of the three annotations belong to the Java extension package: _javax.annotation.Resource_ and _javax.inject.Inject_. The `@Autowired` annotation belongs to the _org.springframework.beans.factory.annotation_ package.

Each of these annotations can resolve dependencies either by field injection or by the setter injection. A simplified but pratical exampe will be used to demonstrate the distinction between the three annotations,, based on  the execution paths taken by each annotation.
## The @Resource Annotation
The _@Resource_ annotation is part of the JSR-250 annotation collection and is packaged with Jakarta EE. This annotation has the following execution paths, listed by precedence:
1. Match by Name
2. Match by Type
3. Match by Qualifier
These execution paths are applicable to both setter and field injection.

## The @Inject Annotation
The _@Inject_ annotation belongs to the JSR-330 annotations collection. This annotation has the following execution paths, list be precedence:
1. Match by Type
2. Match by Qualifier
3. Match by Name

These execution paths are applicable to both setter and field injection. In order to acces the @Inject annotation, the _javax.inject_ library has to be declared as a Gradle or Maven depenedency.

For Gradle
```javascript
testCompile group: 'javax.inject', name: 'javax.inject', version: '1'

```

For Maven
```xml
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
```
## The @Autowired Annotation
The behavior of _@Autowird_ annotation is similar to the _@Inject_ annotation. The only difference is that _@Autowired_ annotation is part of the Spring framework. This annotation has the same execution paths as the _@Inject_ annotation, listed in order of precedence.
1. Match by Type
2. Match by Qualifier
3. Match by Name

These execution paths are applicable to both setter and field injection.

## Applying these Annotations

| Scenarios| @Resource | @Inject | @Autowired|
|----------|-----------|---------|-----------|
|**Application-Wide use of Singletons through polymorphism**. If the design is such that application behaviors are based on implementation of an interface or abstract class, and these behavior are used throughout the application| No | Yes | Yes|
|**Fine-grained application behavior configuration through polymorphism**. If the design is such that the application has complex behavior, each behavior is based on different interfaces/abstract classes, and usage of each of these implementations varies across the application| Yes | No | No|
|**Dependency Injection should be handled solely by the Jakarta EE platform**| Yes | Yes | No|
|**Dependency Injection Should be Handled Solely by the Spring Framework**| No | No | Yes|

# What are Bean Factory and Application Context?
The ApplicationContext is the central interface within a Spring application that is used for providing configuration information to the application.

It implements the BeanFactory. Hence, the ApplicationContext includes all functionality of the BeanFactory and much more. Its main function is to support the creation of big business applications.

Featurs:
- Bean instantiation/ wiring
- Automatic BeanPostProcessor registration
- Automatic BeanFactoryPostProcessor registration
- Convenient MessageSource access (for i18n)
- ApplicationEvent publication

The most commonly used ApplicationContext implementation are -
* FileSystemXMLApplicationContext
* ClassPathXMLApplicationContext
* WebXMLApplicationContext

**@SpringBootApplication** is a convenience annotation that adds all of the following:
- `@Configuration`: Tags the class as source of bean definitions for the application context.
- `@EnableAutoConfiguration`: Tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings. For example, if `spring-webmvc` is on the classpath, this annotation flags the applivation as a web application and activates key behaviors, such as setting up as `DispatchServelt`.
- `@CompactScan`: Tells Spring to look for other components, configuration, and servies in the `com/example` packages, letting it find the controllers.


# Stereotype annotations

## @Component
The `@Component` annotation marks a java class as a bean so the component-scanning mechanism of spring can pick it up and pull it into the application context.

## @Repository
The `@Repository` annotation is a specialization of the `@Component` annotation with similar used and functionality. In addition to importing the DAOs into the DI container, it also makes the unchecked exceptions (thrown from DAO methods) eligible for translation in Spring `DataAccessException`.

## @Service
The `@Service` annotation is also a specialization of the component annotation. It doesn't currently provide any additional behavior over the `@Component` annotation, but it's a good idea to use `@Service` over `@Component` in service-layer classes because it specific intent better. 

## @Controller
The `@Controller` annotation marks a class as a Sprin Web MVC controller. It too is a `@Component` specialization, so beans marked with it are automatically imported into the DI container. When we add the `@Controller` annotation to a class, we can use another annotation ie `@RequestMapping`; to map URLs to instance method of class.

## @Primary
In Spring framework, the `@Primary` annotation is used to give higher preference to a bean, where there are multiple beans of same type.

## @Profile
Using the `@Profile` annotation we are mapping the bean to that particular profile; the annotation simply takes the name of one(or multiple) profiles.

# What is the default scope of a bean?
Spring inclides 7 different Bean scopse
1. **Singleton** : is the default scope for a Bean the one that will be used if nothing else is indicated. 
2. **Prototype** : In the Prototype scope, Spring container will create a new instance of the object described by the bean each time it receives a request. 
3. Request
4. Session
5. Global Session
6. Application
7. Websocket


# What is CDI (Contexts and Dependency Injection)?
CDI (Context and Dependency Injection) is a standard dependency injection framework included in Java EE 6 and higher.

It allows us to manage the lifecycle of stateful components via domain-specific lifecycle contexts and inject components(services) into client objects in a type-safe way.

Spring do not implement CDI.

# What are the important Goals of Spring Boot?
The main goal of Spring Boot Framework is to reduce Developement, Unit Test and integration test time and ease the development of Production ready web applications very easily compared to existing Spring framework which really takes more time.
* To avoid XML Configuration completely.
* To avoid defining more Annotation Configuration.
* To avoid writing lots of import statement.
* To provide some defaults to quick start new projects within no time.
* To provide Opinionated Development approach.

# What is a ViewResolver?
All MVC frameworks for web applications provide a way to address views. Spring provides view resolvers, which enable you to render models in a browser without typing you to a specific view technology. 

# What is Auto Configuration?
Spring Boot auto-configuration attempts automatically configure your Spring application based on jar dependencies that you have added. For example, if HSQLDB is on your classpath, and you have not manually configured any database connection beans, then we will auto-configure as in-memory database.

If you find that specific auto-configure classes are being applied that you don't want you can use the exclude attribute of `@EnableAutoConfiguration` to disable them.
```java
import org.springframework.boot.autoconfigure.*;
import org.springframework.boot.autoconfigure.jdbc.*;
import org.springframework.context.annotation.*;

@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
}
```
If the class is not on the classpath, you can use the `excludeName` attribute of the annotation and specify the fully qualified name instead. Finally, you can also control the list of auto-configuration classes to exclude via the `spring.autoconfigure.exclude` property.

# What are Starter Projects?
Starters are a set of convenient dependency descriptors that you can include in your application. You get a one-stop-shop for all Spring and related technology that you need, without having to hunt through sample code and copy paste loads of dependency descriptors. For example, if you want to get started using Spring and JPA for databases access, just include the `spring-boot-starter-data-jpa` dependency in your project, and you are good to go.

# What is Spring Boot Actuator?
Spring Boot Actuator is a sub-project of the Spring boot framework. It contains the actuator endpoints(the place where resource lives). We can use HTTP and JMX endpoints to manage and monitor the Spring Boot application.

Monitoring our app, gathering metrics, understanding traffic, or the state of our database becomes trivial with this dependency.

# What is a CommandLineRunner and ApplicationRunner?
Spring Boot provides two interfaces; `CommandLineRunner` and `ApplicationRunner`, to run specific pieces of code when an application is fully started. These interface get called just before `run()`

## CommandLineRunner
This interface provides access to application arguments as string array. Let's see the example code for more clarity
```java

@Component
public class CommandLineAppStartupRunner implements CommandLineRunner {
    private static final Logger logger = LoggerFactory.getLogger(CommandLineAppStartupRunner.class);
    @Override
    public void run(String...args) throws Exception {
        logger.info("Application started with command-line arguments: {} . \n To kill this application, press Ctrl + C.", Arrays.toString(args));
    }
}
```

## ApplicationRunner
`ApplicationRunner` wraps the raw application arguments and exposes the `ApplicationArguments` interface, which has many convient methods to get arguments, like `getOptionNames()` to return all the arguments names, `getOptionValues()` to return the argument value, and raw source arguments with method `getSourceArgs()`. Let see an example:
```java
@Component
public class AppStartupRunner implements ApplicationRunner {
    private static final Logger logger = LoggerFactory.getLogger(AppStartupRunner.class);
    @Override
    public void run(ApplicationArguments args) throws Exception {
        logger.info("Your application started with option names : {}", args.getOptionNames());
    }
}
```

Most console applications will only have a single class that implements CommandLineRunner. If your application has multiple classes that implement CommandLineRunner, the order of execution can be specified using Spring's @Order annotation.

# Error handling for Rest with Spring (https://www.baeldung.com/exception-handling-for-rest-with-spring)
Before Spring 3.2, the two main approaches to handling exceptions in a Spring MVC applications were: `HandlerExceptionResolver` ot the `@ExceptionHandler` annotation. Both of these have some clear downsides.

Since 3.2 we've had the `@ControllerAdvice` annotation to address the limitations of the previous two solutions and to promote a unified exception handling throughout a whole application.

Now, Spring 5 introduces the `ResponseStatusException` class. A fast way for basic error handling in our REST APIs. 

All of these do have one thing in common - they deal with the `spearation of concerns` very well. The app can throw exception normally to indicate a failure of some kind - exceptions which will then be handled separately. 

# Fixing NoUniqueBeanDefinitionException
When you do have more than one bean of a given type, you need to tell Spring which bean you wish it to use for dependency injection. If you fail to do so. Spring will throw a `NoUniqueBeanDefinitionException` expection which means there's more than one bean which would fulfill the requirement.

There are two simple ways you can resolve the NoUniqueBeanDefinitionException exception in Spring. You use the `@Primary` annotation, which tell Spring when all other things are equals to select the primary bean over other instances of that type for the autowire requirement.

The second way, is to use the `@Qualifier` annotation. Through the use of this annotation, you can give Spring hints about the name of the bean you want to use. By defailt, the reference name of the bean is typically the lower case class name.

# NoSuchBeanDefinitionException
Exception thrown when a `BeanFactory` is asked for a bean instance for which it cannot find a definition. This may point to a non-existing bean, a non-unique bean, or a manually registered single instance without an associated bean definition.

# What is AOP?
Two important AOP concepts are:
- **Aspect** : Aspect is the concern that we are trying to implement generically. Ex. logging, transaction management. Adivice is the specific aspect of the concern we are implementing.
- **Pointcut**: An expression which determines what are the methods that the Advice should be applied on.

Different types of AOP advices:
1. **Before advice**: Executed before executing the acutal method.
2. **After returning advice**: Executed after executing the actual method.
3. **After throwing advice**: Executed if the actual method call throws an exception.
4.  **After (finally) advice**: Executed in all scenarios (exception or not) after actual method call.
5. **Around advice**: Most powerful advice which can perform custom behavior before and after the method invocation.

# Name some of the design patterns used in Spring Framework?
1. **Factory Design Pattern**: The spring framework uses the factory design pattern for the creation of the objects of beans by using the following two approaches.
    - **Spring Beanfactory Container**
    - **Spring ApplicationContext Container**

2. **Proxy Design Pattern**: In the proxy design pattern, a class is used to represent the functionality of another class. It is an example of a structural pattern. Here, an object is created that has an original object to interface its functioniality to the outer world. Proxy design pattern is widely used in AOP, and remoting.

3. **Singleton Design Pattern**: Singleton design pattern ensures that there will exists only the single instance of the object in the memory that could provide services. In the spring framework, the Single is the default scope and IOC container creates exactly one instance of the object per spring IOC container.

4. **Model View Controller**: It is a design pattern which comes into picture when we use the spring framework for web programming. Spring MVC is known to be a lightweight implementation as controllers are POJOs against traditional servlets which makes the testing of controllers very comprehensive. A controller returns a logical view name and the view selection with the help of a separate ViewResolver. Therefore, Spring MVC controllers can be used along with different view technologies such as JSP etc.

5. **Front Controller Design Pattern**: The front controller design pattern is a technique in software engineering to implement centralized request handling mechanism which is capable of handling all the requests through a single handler. Such handler can perform the authentication, authorization and logging or request tracking (i.e. passs the requests to the corresponding handlers). Spring framework provides support for the DispatcherServlet that ensure to dispatch an incoming request to your controllers.

6. **View Helper**: Spring framework provides a large number of custom JSP tags as well as velocity marcos which assits in the separation of code from the presentation(i.e views).

7. **Template method**: Spring framework provides a number of templates to kick start work and complete that piece of work as the best programming practice such as opening and closing connection for JDBC or JMS, etc. Eg JdbcTemplate, JmsTemplate, and JpaTemplate.

# Spring DispatcherServlet - how it works?
`DispatcherServlet` acts as `front-controller` for Spring based web applications. It provides a mechanism for request processing where actual work is performed by configurable, delegate components. It is inherited from `javax.servlet.http.HttpServlet`, it is typically configured in the `web.xml` file.


# Circular dependencies
It happens when a bean A depends on another bean B and the bean depends on the bean A as well:
Bean A --> Bean B  --> Bean A

Of course, we could have more beans implied:
Bean --> Bean B --> Bean C --> Bean D --> Bean E --> Bean A

When Spring context is loading all the beans it tries to create beans in the order needed for them to work completely. For instance, if we didn't have a circular dependency like the following case:

Bean A --> Bean B --> Bean C

Spring will create bean C then create Bean B (and inject bean C into it) then create bean A (and inject bean B into it). But, when having a circular dependency spring cannot decide which of the bean should be created first, since they depend on one another. In these case , Spring will raise a BeanCurrentlyInCreationException while loading context. 

It can happen in Spring when using **constructor injection**: if you use other types of injections you should not find this problem since the dependencies will be injected when they are needed and not on the context loading.

We will show some of the most popular wats to deal with this problem.
1.  **Redesign** - When you have circulart dependency, its likely you have a design problem and the responsibilities are not well separated.  You should to redesign the components properly so their hierarchy is well designed and there is not need for circular dependencies.
2. **@Lazy** - A simple way to break the cycle is saying Spring to initialize one of the beans lazily. That is: instead of fully initializing the bean, it will create a proxy to inject it into the other bean. The injected bean will only be fully created when it's first needed.
3. **Use Setter / Field Injection** - One of the most popular workarounds and also what Spring documentation proposes, is using setter injection.
4. **Use @PostConstruct** - Another way to break the cycle is injecting a dependency using `@Autowirded` on one of the beans, and then use a method annotated with `@PostConstruct` to set the other dependency.

# @ConfigurationProperties 
In Spring boot, the `@ConfigurationProperties` annotation allows us to map the resource file such as properties or YAML files to Java Bean object. This annoatation is applied to a class or a `@Bean` method in a `@Confifuration` class to map or variable the external properties or YAML files.

# Spring Boot Actuator ?
Spring Boot Actuator is a sub-project of the Spring boot framework. It contains the actuator endpoints(the place where the resource live). We can use HTTP and JMX endpoints to manage and monitors the Spring Boot application.

# Peristence Context

> Official Definition
>
> An EntityManager instance is associated with a persistence context. A persistence context is a set of entity instances in which for any persistent entity identity there is a unique entity instance. Within the persistent context, the entity instances and their lifecycle are managed. The EntityManager API is used to create and remove persistent entity instances, to find entities by their primary key and to query over entities.

The persistent context is the first-level cache where all the entities are fetched from the datanase or saved to the database. It sit between our application and persistent storage.


