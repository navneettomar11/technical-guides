# Reason: Failed to determine a suitable driver class
Now, suppose we have a Spring Boot project, and we've added the `spring-data-starter-jpa` dependency and a `MySQL JDBC driver` to our _pom.xml_.
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```
But, when we run the application, it fails with the error:
```
Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded
  datasource could be configured.

Reason: Failed to determine a suitable driver class
```
**By design, Spring Boot auto-configuration tries to configure the beans automatically based on the dependencies added to the classpath.**
And, since we have the JPA dependency on our classpath, Spring Boot tries to automatically configure a JPA Datasource. The problem is, **we haven't given Spring the information it needs to perform the auto-configuration.**
For example, we haven't defined any JDBC connection properties, and we'll need to do so when working with external databases like MySQL and MSSQL. On the other hand, we won't face this issue with in-memory databases like H2 since they can create data source without all this information.

## Solutions
1. **Define the DataSource Using properties**
Since the issue occurs due to the missing database connection, we can solve the problem simply by providing the data source properties.

First, let's defines the data source properties in the _application.properties_ file of our project.
```
spring.datasource.url=jdbc:mysql://localhost:3306/myDb
spring.datasource.username=user1
spring.datasource.password=pass
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
2. **Define the DataSource Programmatically**
Alternatively, we can define our data source programmatically, by **using the utility builder class DataSourceBuilder**. We need to provide the database URL, username, password, and the SQL driver information to create our data source:

```java
@Configuration
public class DatasourceConfig {
    @Bean
    public DataSource datasource() {
        return DataSourceBuilder.create()
          .driverClassName("com.mysql.cj.jdbc.Driver")
          .url("jdbc:mysql://localhost:3306/myDb")
          .username("user1")
          .password("pass")
          .build();
    }
}
```
3. **Exclude DataSourceAutoConfiguration**
Let's see how to **prevent Spring Boot from auto-configuring the data source**.
The class _DataSourceAutoConfiguration_ is the base class for configuring a data source using the _spring.datasource*.properties_.
First, we can disable the auto-configuration using **spring.autoconfigure.exclude.property** in our application.properties.
```
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```
Or, wecan use the exlude attribute on our @SpringBootApplication or @EnableAutoConfiguration annotation.
```java
@SpringBootApplication(exclude={DataSourceAutoConfiguration.class})
```
> Ideally, we should provide the data source information and use the exclude option only for testing.


# Circular view path : would dispatch back to the current handler URL
Whe  you don't declare a `ViewResolver`, Spring register a default `InternalResourceViewResolver` which creates instances of `JstlView` for rendering the `View`.
