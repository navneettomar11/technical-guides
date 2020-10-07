# OAuth2 Boot

## 1. Authorization Server

### 1.1 Do I need to Stand up my own Authorization server?
You need to stand-up your own authorization server if:
- You want to delegate the operations of sign-n, sign-out and password recovery to a separate service (also called identity federation) that you want to manage yourself and
- You want to use the OAuth2.0 protocol for this separate service to coordinate with other services.

### 1.2 Dependencies
To use the auto-configuration features in this library, you need `spring-security-oauth2`, which has the OAuth2.0 primitives and `spring-security-oauth2-autoconfigure`. Note that you need to specify the version for `spring-security-oauth2-autoconfigure`, since it is not managed by Spring boot any longer, though it should match Boot's version anyway.

### 1.3 Minimal OAuth2 Boot configuration
Creating a minimal Spring Boot authorization server consists of three basic steps:
1. Including Dependencies
2. Including the `@EnableAuthorizationServer` annotation.
3. Specifying at least one clientId and secret pair.

#### 1.3.1 Enabling Authorization server
Similar to other Spring Boot `@Enable` annotations, you can add the `@EnableAuthorizationServer` annotation to the class that contains your `main` methof, as the following example shows:
```java
```
