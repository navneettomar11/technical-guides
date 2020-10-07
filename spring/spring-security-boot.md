# Creating your Spring Security configuration
```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http.
    authorizeRequests()
    .andMatchers("/css/**","/index").permitAll()    // 1
    .andMatchers("/user/**").hasRole("USER")        //2
    .and()
    .formLogin()
    .loginPage("/login").failureUrl("/login-error");    // 3
  }

  @Autowired
  public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
    auth.inMemoryAuthentication().withUser("user").password("password").roles("USER");
  }
}
```
1. requests matched against /css/** and index are fully accessible
2. requests matched against /user/** required a user to be authenticated and must be associated to the USER role.
3. form-based authentication is enabled with a custom login page and failure url.
