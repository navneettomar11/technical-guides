# Web Application Security (OSWAP Top Ten - 2017)

# Injection
- **What is it?** Untrusted user input is interpreted by server and executed.
- **What is the impact?** Data can be stolen, modified or deleted.
- **How to prevent?** 
    * Reject untrusted/invalid input data.
    * Use latest frameworks
    * Typically found by penetration tester / secure code review.

# Broken Authentication and Session Management
- **What is it?** Incorrectly build auth, and session managment , schema that allow an attacker to impersonate another user.
- **What is the impact?** Attacker can take identity of victim.
- **How to prevent?** Don't develop your own authentication schemes.
    * Use open source frameworks that are actively maitained by the community.
    * Use strong passwords (incl. upper, lower, number, special characters)
    * Required current credentials when sensitive information is requested or changed.
    * Multi-factor authentication(e.g. sms, password, fingerprint, iris scan etc).
    * Log out or expire session after X amount of time.
    * Be careful with `remember me` functionality.

# Cross-Site Scripting (XSS)
- **What is it?** Untrusted user input is interpreted by browser and executed
- **What is the impact?**  Hijack user sessions, deface web sites, change content
- **How to prevent?**
    * Escape untrusted input data,
    * Latest UI framework 

# Broken Access Control
- **What is it?** Restrictions on what authenticated users are allowed to do are not properly enforced.
- **What is the impact?** Attackers can assess data, view sensitive files and modify data.
- **How to prevent?**
    * Application should not solely rely on user input; check access rights on UI level and server level for request to resourve (e.g. data)
    * Deny access by default.

# Security Misconfiguration
 - **What is it?** Human mistake of misconfigurating the system (e.g. providing a user with a default password)
 - **What is the impact?** Depends on the misconfiguration. Worst misconfiguration could result in loss of the system.
 - **How to prevent?**
    * Force change of default credentials
    * Least privilege: turn everything off by default (debugging, admin, interface,etc.)
    * Static tools that scan code for default settings.
    * Keep patching, updating and testing the syste .
    * Regularly audit system deployment in production system.

# Sensitive Data Exposure
- **What is it?** Sensitive data is exposed, e.g. social security numbers, password, health records
- **What is the impact?** Data that are lost exposed or corrupted can have severe impact on business continuity
- **How to prevent?**
    * Always obscure data (credit card numbers are almost always obscured)
    * Update cryptographic algorithm (MD5, DES, SHA-0 and SHA-1 are insecure)
    * Use salted encryption on storage of passwords.

# Insufficient Attack Protection
- **What is it?** Applications that are attacked but do not recognize it as an attack, letting the attacker again and again.
- **What is the impact?** Leak of data, descrease application availability
- **How to prevent?**
    * Detect and log normal and abnormal use of application
    * Respond by automatically blocking abnormal users or range of IP addresses.
    * Patch abnormal use quickly.

# Cross-site request forgery(CSRF)
- **What is it?** An attack that forces a victim to execute unwanted actions on a web application in which they're currently authenticated.
- **What is the impact?** Victim unknowingly executes transactions.
- **How to prevent?**
    * Reauthenticate for all critical actions (e.g. transfer money)
    * Include hidden token in request.
    * Most web frameworkds have built-in CSRF protecting, but isn't enabled by default!

# Using Components with known Vulnerabilities
- **What is it?** Third-party components that the focal system use (e.g. authentication frameworks)
- **What is the impact?** Dependening on the vulnerability it could range from subtle to seriously bad.
- **How to prevent?** 
    * Always stay current with third-party components
    * If possible, follow best pratice of virtual patching.

# Underprotected APIs
- **What is it?** Applications expose rich connectivity options through APIs, in the browser to a user. These APIs are often unprotected and contain numberous vulnerabilities 
- **What is the impact?** Data theft, corruption, unathourized access, etc.
- **How to prevent?** 
    * Ensure secure communication between client browser and server API.
    * Rejected untrusted/invalid input data.
    * Use latest framework
    * Vulnerabilities are typically found by penetration testers and secure code reviewers.

# Defense in Depth  
[!Defense in Depth](assets/defence-in-depth-2054.jpg)

# STRIDE
## Why ? 
- Examine what can go wrong
- What are you going to do about it
- Determine whether you doing a good job.

Stand for 
- Spoofing - pretending of
- Tempering - removing the traces
- Repudiation - 
- Information disclosure
- Denail of service
- Elevation of privilege

# Secure Development Process

## Microsoft security development lifecycle (MS SDL)
[!SDL](assets/5554.SDL_Steps.jpg)

