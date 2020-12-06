# Angularjs vs Angular
|Feature|Angularjs|Angular|
|-------|---------|-------|
|Architecture| Support MVC design model| Uses component and directives|
|Language| Recommended Language: Javascript| Recommend Language: Typescript|
|Mobile support| Doesn't provide any mobile support| Provides mobile support|

# Advantage of using Angular
1. It supports two-way data-binding
2. It follow MVC pattern architecture
3. It supports static template and Angular template
4. You can add a custom directive
5. It also support RESTfull services.
6. Validation are supported.
7. Client and server communication is facilitated
8. Support for dependency injection.
9. Has strong feature like Event Handlers, Animation, etc.

# Constructor vs ngOnInit 
The `constructor` function comes with every class, constructors are not specific to Angular but are concept derived from Object oriented designs. The constructor creates an instance of the component class.

The `ngOnInit` function is one of an Angular component's life cycle methods. Life cycle methods (or hooks) in Angular components allow you to run a piece of code at different stages of the life of a component.

Unlike the `constructor` method, `ngOnInit` method comes from an Angular interface (`OnInit`) that the component needs to implement in order to use this method. The `ngOnInit` method is called shortly after the component is created.

For most people coming with a good background in Object Oriented programming, it makes sense to insert dependencies into the `constructor` and also put all the component initialization code inside the `constructor` body. While the former works fine, the latter results in an Angular gotcha when you realize that all your initialization code placed in the `constructor` body is not executed.

This is because the constructor is called to initialize the class and not the component. The `constructor` is called before `ngOnInit`, at this point the component hasn't been created yet, only the component class has being instantiated thus your dependencies are brought in, but your initialization code will not run.

To ensure your initialization code runs, simply put it in the `ngOnInit` function which guarantees you that the component has already being created.


# AOT vs JIT
JIT  - compile typescript just in time for executing it.
- Compiled in the browser
- Each file compiled separately
- No need to build after changing your code and before reloading the browser page.
- Suitable for local development

AOT - Compile Typescript during build phase.
- Compiled by the machine itself, via the command line.
- All code compiled together, inlining HTML/CSS in the scripts.
- No need to deploy the compiler
- More secure, original source not disclosed.
- Suitable for production builds.

# How are observable different from promise?
The first difference is that Observable is lazy whereas a Promise is eager.

|Promise| Observable|
|Emit a single value| Emit multiple values over period of times|
|Not Lazy| Lazy. An observable is not called until we subscribe to the observable|
|Cannot be cancelled| Can be cancelled by using the unsubscribe() method|
|| Observable provides operators like map, forEach, filter, reduce, retry, retryWhen, etc|

# Microfrontend
Micro frontend are new pattern where web application UI (front ends) are composed from semi-idependent fragments can be build by differently teams using different technologies. Micro frontend architecture ressemble back-end archiecture where back ends are composed from semi-independent microservices.

