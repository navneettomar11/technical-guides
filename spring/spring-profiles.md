# Profiles
Spring Profies provide a way to segregate parts of your application configuration and make it only available in certain environments. Any `@Component` or `@Configuration` can be marked with `@Profie` to limit when it is loaded.

```java
@Configuration
@Profile('Production')
public class ProductionConfiguration {
    // ...
}
```
In the normal Spring way you can use a `spring.profile.active` `Environment` property to specify which profile are active. You can specify the property in any of the usual ways, for example you could include it in your `application.properties`.

> spring.profile.active = dev, hsqldb

or specify on the command line using the switch `--spring.profiles.active=dev, hsqldb`

## Adding active profiles
The `sprinng.profiles.active` property follows the same ordering  rules as other properties, the highest `PropertySource` will win. This mean that you can specify active profiles in `application.properties` then replace them using the command line switch.

Sometimes it is useful to have profle specific properties that add to the active profile rather than replace them. The `spring.profiles.include` property can be used to unconditionally add active profiles. The `SpringApplication` entry point also have a Java API for setting additional profiles.(i.e. on top of those activated by the `spring.profile.active` property). see the `setAdditionalProfiles()` method.

For example, when an application with following properties is run using the switch `--spring.profiles.active=prod` the `proddb` and `prodmq` profiles will also be activated.
```
---
my.property: fromyamlfile
----
spring.profiles: prod
spring.profiles.include: proddb, prodmq
