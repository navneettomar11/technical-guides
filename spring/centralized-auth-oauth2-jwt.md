# Centralized Authorization with OAuth2 + JWT using Spring Boot 2

## OAuth 2 Terminology
- **Resource Owner** : The user who authorizes an application to access his account. The access is limited to the `scope`.
- **Resource Server**: A server that handles authenticated requests after the `client` has obtained an `access token`.
- **Client**: An application that access protected resources on behalf of the resource owner.
- **Authorization Server**: A server which issues access token after successfully authenticating a `client` and `resource owner`, and authorizing the request.
- **Access Token**: A unique token used to access protected resources.
- **Scope**: A permission
- **JWT**: JSON Web Token is a method for representing claims securely between two parties as defined in `RFC 7519`
- **Grant type**: A `grant` is a method of accquring an access token.

## Authorization server
To build our `Authorization Server` we'll be using Spring Security 5.x through Spring Boot 2.1.x

### Dependencies
You can go to start.spring.io and generate a new project and then add the following dependencies:
```xml
<dependencies>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.security.oauth.boot</groupId>
    <artifactId>spring-security-oauth2-autoconfigure</artifactId>
    <version>2.1.2.RELEASE</version>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
</dependencies>
```

### Database
Here you can find a reference OAuth2 SQL schema required by Spring Security.
```SQL
CREATE TABLE IF NOT EXISTS oauth_client_details (
  client_id VARCHAR(256) PRIMARY KEY,
  resource_ids VARCHAR(256),
  client_secret VARCHAR(256) NOT NULL,
  scope VARCHAR(256),
  authorized_grant_types VARCHAR(256),
  web_server_redirect_uri VARCHAR(256),
  authorities VARCHAR(256),
  access_token_validity INTEGER,
  refresh_token_validity INTEGER,
  additional_information VARCHAR(4000),
  autoapprove VARCHAR(256)
);

CREATE TABLE IF NOT EXISTS oauth_client_token (
  token_id VARCHAR(256),
  token BLOB,
  authentication_id VARCHAR(256) PRIMARY KEY,
  user_name VARCHAR(256),
  client_id VARCHAR(256)
);

CREATE TABLE IF NOT EXISTS oauth_access_token (
  token_id VARCHAR(256),
  token BLOB,
  authentication_id VARCHAR(256),
  user_name VARCHAR(256),
  client_id VARCHAR(256),
  authentication BLOB,
  refresh_token VARCHAR(256)
);
CREATE TABLE IF NOT EXISTS oauth_refresh_token (
  token_id VARCHAR(256),
  token BLOB,
  authentication BLOB
);

CREATE TABLE IF NOT EXISTS oauth_code (
  code VARCHAR(256), authentication BLOB
);
```
And then add the following entry
```SQL
-- The encrypted client_secret it `secret`
INSERT INTO oauth_client_details (client_id, client_secret, scope, authorized_grant_types, authorities, access_token_validity)
  VALUES ('clientId', '{bcrypt}$2a$10$vCXMWCn7fDZWOcLnIEhmK.74dvK1Eh8ae2WrWlhr2ETPLoxQctN4.', 'read,write', 'password,refresh_token,client_credentials', 'ROLE_CLIENT', 300);
```
Below here you can find the `User` and `Authority` reference SQL schema used by Spring's
`org.springframework.security.core.userdetails.jdbc.JdbcDaoImpl`.
```SQL
CREATE TABLE IF NOT EXISTS users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(256) NOT NULL,
  password VARCHAR(256) NOT NULL,
  enabled TINYINT(1),
  UNIQUE KEY unique_username(username)
);

CREATE TABLE IF NOT EXISTS authorities (
  username VARCHAR(256) NOT NULL,
  authority VARCHAR(256) NOT NULL,
  PRIMARY KEY(username, authority)
);
```
Same as before add the following entries for the user and its authority.
```SQL
INSERT INTO users (id, username, password, enabled) VALUES (1, 'user', '{bcrypt}$2a$10$cyf5NfobcruKQ8XGjUJkEegr9ZWFqaea6vjpXWEaSqTa2xL9wjgQC', 1);
INSERT INTO authorities (username, authority) VALUES ('user', 'ROLE_USER');
```
### Spring Security configuration
Add the following Spring configuration classes
```java
import org.springframework.context.annotation.Bean;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.jdbc.JdbcDaoImpl;
import org.springframework.security.crypto.factory.PasswordEncoderFactories;
import org.springframework.security.crypto.password.PasswordEncoder;

import javax.sql.DataSource;

@EnableWebSecurity
public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {

    private final DataSource dataSource;

    private PasswordEncoder passwordEncoder;
    private UserDetailsService userDetailsService;

    public WebSecurityConfiguration(final DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Override
    protected void configure(final AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService())
                .passwordEncoder(passwordEncoder());
    }

    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        if (passwordEncoder == null) {
            passwordEncoder = PasswordEncoderFactories.createDelegatingPasswordEncoder();
        }
        return passwordEncoder;
    }

    @Bean
    public UserDetailsService userDetailsService() {
        if (userDetailsService == null) {
            userDetailsService = new JdbcDaoImpl();
            ((JdbcDaoImpl) userDetailsService).setDataSource(dataSource);
        }
        return userDetailsService;
    }

}
```
Let's understand the meaning of the annotation's and configurations used in the `SecurityConfig` class -
1. **@EnableWebSecurity**: This is the primary spring security annotation that is used to enable web security in project.
2. **@EnableGlobalMethodSecurity**: This is used to enable method level security based on annotations.You can use following three type annotations for securing your methods -
  - **secureEnabled**: It enables the `@Secured` annotation using which you can protect your controller/service methods like so -
  ```java
  @Secured("ROLE_ADMIN")
  public User getAllUsers() {}
  ```
  - **jsr250Enabled**: It enables the `@RoleAllowed` annotation that can be used like this -
  ```java
    @RolesAllowed("ROLE_ADMIN")
    public User createUser() {}
  ```
  - **prePostEnabled**: It enables more complex expression based access control syntax with `@PreAuthorize` and `@PostAuthorize` annotation -
  ```java
  @PreAuthorize("isAnonymous()")
  public boolean isUsernameAvailable() {}

  @PreAuthorize("hasRole('USER')")
  public User createUser() {}  
  ```
3. **WebSecurityConfigurerAdapter**: This class implements Spring Security `WebSecurityConfigurer` interface. It provides default security configurations and allows other classes to extend it and customize the security configuration by overriding its methods.
4. **CustomUserDetailsService**: To authenticate a User or perform various role-based checks, Spring security needs to load user details somehow.

If your using Spring Boot the `DataSource` object will be autoconfigured and you can jist inject it to the class instead of defining it yourself. It needs to be injected to the ``
