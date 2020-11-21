# Dependency injection in Angular
Dependency injection (DI), is an important application design pattern. Angular has it won DI framework, which is typically used in the design of Angular applications to increase and modularity.

Dependencies are services or objects that class needs to perform its function. DI is a coding pattern in which a class asks for dependencies from external sources rather than creating them itself.

In Angular, the DI framework provides declared dependencies to a class when that class is instantiated. This guide explains how DI works in Angular, and how you use it to make your apps flexible, efficient and robust as well as testable and maintainable

Start by reviewing this simplified version of the heroes feature from the The Tour of Heroes. This simple version doesn't use DI; we'll walk through converting it to do so.

```javascript
//heroes.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-heroes',
  template: `
    <h2>Heroes</h2>
    <app-hero-list></app-hero-list>
  `
})
export class HeroesComponent { }

//hero-list.component.ts
import { Component } from '@angular/core';
import { HEROES } from './mock-heroes';

@Component({
  selector: 'app-hero-list',
  template: `
    <div *ngFor="let hero of heroes">
      {{hero.id}} - {{hero.name}}
    </div>
  `
})
export class HeroListComponent {
  heroes = HEROES;
}
//hero.ts
export interface Hero {
  id: number;
  name: string;
  isSecret: boolean;
}

//mock-heroes.ts
import { Hero } from './hero';

export const HEROES: Hero[] = [
  { id: 11, isSecret: false, name: 'Dr Nice' },
  { id: 12, isSecret: false, name: 'Narco' },
  { id: 13, isSecret: false, name: 'Bombasto' },
  { id: 14, isSecret: false, name: 'Celeritas' },
  { id: 15, isSecret: false, name: 'Magneta' },
  { id: 16, isSecret: false, name: 'RubberMan' },
  { id: 17, isSecret: false, name: 'Dynama' },
  { id: 18, isSecret: true,  name: 'Dr IQ' },
  { id: 19, isSecret: true,  name: 'Magma' },
  { id: 20, isSecret: true,  name: 'Tornado' }
];
```
HeroesComponent is the top-level heroes component. It only purposes is to display `HeroListComponent`, which displays a list of hero names.

This version of the HeroListComponent gets heroes from the HEROES arrat, an in-memory collection defined in a separate mock-heroes file.

```javascript
export class HeroListComponent {
  heroes = HEROES;
}
```

This approach works for prototyping but is not robust or maintainable. As soon as you try to test this component or get heroes from a remote server, you have to change the implementation of HeroListComponent and replace every use of the HEROES mock data.

## Create and register an injectable service
The DI framework lets you supply data to a component from an injectable service class, defined in its own file. To demonstrate we'll create an injectable service class that provides a list of heroes and register that class as a provider of that service.
### Create an injectable service class
The Angular CLI can generate a new HeroService class in the src/app/heroes with this command:

> ng generate service heroes/hero

```javascript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class HeroService {
  constructor() { }
}
```

The @Injectable() is an essential ingredient in every Angular service definition. The rest of the class written to expose a getHeroes method that returns the same mock data as before. (A real app would probably get its data asynchronously from a remote server, but we'll ignore that to focus on the mechanics of injecting the service.)

```javascript
import { Injectable } from '@angular/core';
import { HEROES } from './mock-heroes';

@Injectable({
  // we declare that this service should be created
  // by the root application injector.
  providedIn: 'root',
})
export class HeroService {
  getHeroes() { return HEROES; }
}
```
### Configure an injector with a service provider
The class we have created provide a service. The @Injectable() decorator marks it as a service that can be injected, but Angular can't actually inject it anywhere until you configure an Angular dependency injector with a provider  of that service.

The injector is responsible for creating service instances and injecting them into classes like HeroListComponent. You rarely create an Angular injector yourself. Angular creates injectors for you as it executes the app, starting with the root injector that it creates during the bootstrap process.

A provider tells an injector how to create the service. You must configure an injector with a provider before that injector can create a service(or provide any other kind of dependency).

A provider can be the service class itself, so that the injector can use new to create an instance. You might also define more than one class to provide the same service in different ways, and configure different injectors with different providers.

> Injectors are inherited, which means that if a given injector can't resolve a dependency, it ask the parent injector to resolve it. A component can get services from its own injector, from the injectors of its component ancestors, from the injector of its parent NgModule, or from the root injector.

You can configure injectors with providers at different levels of your app, by setting a metadata value in one of three places:
- In the @Injectable() decorator for the service itself.
- In the @NgModule() decorator for an NgModule
- In the @Component() decorator for an Component

The @Injectable() decorator has the `providedIn` metadata option, where you can specify the provider of the decorated service class with the `root` injector, or with the injector for a specific NgModule.

The @NgModule() and @Component() decorators have the providers metadata option, where you can configure providers for NgModule-level or component-level injectors.

> Components are directives and the providers option is inherited from @Directive(). You can also configured providers for directives and pipes at the same level as the component.

### Injecting Services
In order for HeroListComponent to get heroes from HeroService, it needs to ask for HeroService to be injected, rather than creating its own HeroService instance with new.

You can tell Angular to inject a dependency in a component's constructor by specifying a constructor parameter with the depenency type. Here's the HeroListComponent constructor, asking for the HeroService to be injected.

```javascript
constructor(heroService: HeroService)
```
Of course, HeroListComponent should do something with the injected HeroService. Here's the revised component, making use of the injected service, side-by-side with the previous version for comparison.

```javascript
//hero-list.component(with DI)
import { Component } from '@angular/core';
import { Hero } from './hero';
import { HeroService } from './hero.service';

@Component({
  selector: 'app-hero-list',
  template: `
    <div *ngFor="let hero of heroes">
      {{hero.id}} - {{hero.name}}
    </div>
  `
})
export class HeroListComponent {
  heroes: Hero[];

  constructor(heroService: HeroService) {
    this.heroes = heroService.getHeroes();
  }
}

//hero-list.component(without DI)
import { Component } from '@angular/core';
import { HEROES } from './mock-heroes';

@Component({
  selector: 'app-hero-list',
  template: `
    <div *ngFor="let hero of heroes">
      {{hero.id}} - {{hero.name}}
    </div>
  `
})
export class HeroListComponent {
  heroes = HEROES;
}
```
HeroService must be provided in some parent injector. The code in HeroListComponent doesn't depend on where HeroService comes from. If you decided to provide HeroService in AppModule, HeroListComponent wouldn't change.

## Injector hierarchy and service instances
Service are singletons within the scope of an injector. That is, there is at most one instance of a service in a given injector.

There is only one root injector for an app. Providing UserService at the root or AppModule level means it is registered with the root injector. There is just one UserService instance in the entire app and every class that injects UserService gets this service instance unless you configure another provider with a child injector.

Angular DI has a hierarchical injection system, which means that nested injectors can create their own service instances. Angular regularly created nested injectors. Whenever Angular creates a new instance of a component that has provider specified in @Component(), it also creates a new child injector for the instance. Similarly, when a new NgModule is lazy-loaded at run time. Angular can create an injector for it with it own providers.

Child modules and component injectors are independent of each other, and create their own separate instances of the provided services. When Angular destroys an NgModule or component instance, it also destroys that injector and the injector's service instances.

Thanks to injector inheritance, you can still inject application-wide services into these components. A component's injector is a child its parent component's injector and inherits from all ancestor injectors all the way back to the application's root injector. Angular can inject a service provided by an injector in that lineage.

For example, Angular can inject HeroListComponent with both HeroServie provided in the HeroComponent and the UserService provided in AppModule.

## Testing component with dependencies
Designing a class with dependency injection makes the class easier to test. Listing dependencies as constructor parameters may be all you need to test application parts effectively.

For example, you can create a new HeroListComponent with a mock service that you can manipulate under test.

```javascript
const expectedHeroes = [{name: 'A'}, {name: 'B'}]
const mockService = <HeroService> {getHeroes: () => expectedHeroes }

it('should have heroes when HeroListComponent created', () => {
  // Pass the mock to the constructor as the Angular injector would
  const component = new HeroListComponent(mockService);
  expect(component.heroes.length).toEqual(expectedHeroes.length);
});
```
## Services that need other services
Services can have their own dependencies. HeroService is very simple and doesn't have any dependencies of its own. Suppose, however that you want it to report its activities through a logging service. You can apply the same constructor injection pattern, adding a constructor that takes a Logger parameter.

Here is the revised HeroService that injects Logger, side by side with the previous service for comparison
```javascript
//hero.service.ts
import { Injectable } from '@angular/core';
import { HEROES } from './mock-heroes';
import { Logger } from '../logger.service';

@Injectable({
  providedIn: 'root',
})
export class HeroService {

  constructor(private logger: Logger) {  }

  getHeroes() {
    this.logger.log('Getting heroes ...');
    return HEROES;
  }
}
//hero.service.ts (v1)
import { Injectable } from '@angular/core';
import { HEROES } from './mock-heroes';

@Injectable({
  providedIn: 'root',
})
export class HeroService {
  getHeroes() { return HEROES; }
}
//logger.service
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class Logger {
  logs: string[] = []; // capture logs for testing

  log(message: string) {
    this.logs.push(message);
    console.log(message);
  }
}
```
The constructor asks for an injected instance of Logger and stores it in a private field called logger. The getHeroes() method logs a message when asked to fetch heroes.

Notice that the Logger service also has the @Injectable() decorator even though it might not need it own dependencies. In fact, the @Injectable() decorator is required for all services.

When Angular creates a class whose constructor has parameters, it look for type and injection metadata about those parameters so that it can inject the correct service. If Angular can't find tha parameter information, it throw an error. Angular can only find the parameter information if the class has a decorator of some kind. The @Injectable() decorator is the standard decorator for service classes.

> The decorator requirment is imposed by Typescript. Typescript normally discards parameter type information when it transpiles the code to Javascript. Typescript preserves this information if the class has a decorator and the `emitDecoratorMetadata` compiler option is set true in Typescript tsconfig.json configuration file. The CLI configures tsconfig.json with `emitDecoratorMetadata`: true.
>
> This means you're responsible for putting @Injectable() on your service classes.

## Dependency injection tokens
When you configure an injector with a provider, you associate that provider with a DI token. The injector maintain an internal token-provider map that it reference when asked for a dependency. The token is the key to the map.

In simple examples. the dependency value is an instance, and the class type servers as its own lookup key. Here you get a HeroService directly from the injector by supplying the HeroService type as the token.
```javasript
heroService: HeroService;
```

The behavior is similar when you write a constructor that requires an injected class-based dependency. When you define a constructor parameter with the HeroService class type. Angular knowns to inject the service associated with that HeroService class token.
```javascript
constructor(heroService: HeroService)
```
Many dependency values are provided by classes but not all. The expanded provide object let you associated different kinds of providers with a DI token .

## Optional dependencies
HeroService requires a logger, but what if it could get by without one?

When a component or service declares a dependency, the class constructor takes that dependency as a parameter. You can tell Angular that the dependency is optional by annotating the constructor parameter with @Optional().
```javascript
import { Optional } from '@angular/core';
...
constructor(@Optional() private logger?: Logger) {
  if (this.logger) {
    this.logger.log(someMessage);
  }
}
```
When using @Optional(), your code must be prepared for a null value. If you don't reister a logger provider anywhere, the injector sets the value of logger to null.

> @Inject() and @Optional() are parameter decorators. They alter the way the DI framework provides a dependency, by annotating the dependency paramete on the constructor of th class that requires the dependency.

# Dependency Providers
A dependency provider configures an injector with a DI token which that injector use to provide the concrete, runtime version of a dependency value. The injector relies on the provider configuration to create instances of the depenedencies that injects into components, directives, pipes and other services.

You must configure an injector with a provider or it won't know how to create the dependency. The most obvious way for an injector to create an instance of a service class is with the class itself. If you specify the service class itself as the provider token the default behavior is for the injector to instantiate that class with new.

In the following typical example, the Logger class itself provides a Logger instance.
```javascript
providers: [Logger]
```
You can however, configure an injector with an alternative provider, in order to deliver some other object that provides the needed logging functionalility. For instance:
- You can provide a substitue class.
- You can provide a logger-like object.
- Your provider can call a logger factory function.

## The Provider object literal
The class-provider syntax is a shorthand expression that expands into a provider configuration, defined by the Provider interface. The following code snippets shows how a class that is given as the providers value is expanded into a full provider object.
```javascript
providers: [Logger]

[{ provide: Logger, useClass: Logger }]
```
The expanded provider configuration is an object literal with two properties.
- The provide property holds the token that serves as te key for both locating a dependency value and configuring the injector.
- The second property is a provider definition object, which tells the injector how to create the dependency value. The provider-definition key can be useClass, as in the example. It can also be useExisting, useValue, useFactory. Each of these keys provides a different type of dependency.

## Alternative class providers
Different classes can provide the same service. For example, the following code tells the injector to return a BetterLogger instance when the component ask for a logger using the Logger token.

```javascript
[{ provide: Logger, useClass: BetterLogger }]
```
### Class providers with dependencies
Another class, EvenBetterLogger, might display the user name in the log message. This logger gets the user from an injected UserService instance.

```javascript
@Injectable()
export class EvenBetterLogger extends Logger {
  constructor(private userService: UserService) { super(); }

  log(message: string) {
    const name = this.userService.user.name;
    super.log(`Message to ${name}: ${message}`);
  }
}
```
The injectors need providers for both this new logging service and its dependent UserService. Configure this alternative logger with the useClass provider-definition key, like BetterLogger. The following array specifies both providers in the providers metadata option of the parent module or component.

```javascript
[ UserService,
  { provide: Logger, useClass: EvenBetterLogger }]
```
### Aliased class providers
Suppose an old component despends upon the OldLogger class. OldLogger has the same interface as NewLogger, but for some reason you can't update the old component to use it.

When the old component logs a message with OldLogger, you want the singleton instance of NewLogger to handle it instead. In this case, the dependency injector should inject that singleton instance when a component asks for either the new or the old logger. OldLogger, should be an alias for NewLogger.

If you try alias OldLogger to NewLogger with useClass, you end up with two different NewLogger instance in your app.
```javascript
[ NewLogger,
  // Not aliased! Creates two instances of `NewLogger`
  { provide: OldLogger, useClass: NewLogger}]
```
To make sure there is only one instance of NewLogger, alias OldLogger with the useExisting option
```javascript
[ NewLogger,
  // Alias OldLogger w/ reference to NewLogger
  { provide: OldLogger, useExisting: NewLogger}]
```
## Value providers
Sometimes it's easier to provide a ready-made object rather than ask the injector to create it from a class. To inject an object you have already created, configure the injector with the useValue option.

The following code defines a variable that creates such an object to play the logger role.
```javascript
// An object in the shape of the logger service
function silentLoggerFn() {}

export const SilentLogger = {
  logs: ['Silent logger says "Shhhhh!". Provided via "useValue"'],
  log: silentLoggerFn
};
```
THe following provider object uses the useValue key to associate the variable with the Logger token.
```javascript
[{ provide: Logger, useValue: SilentLogger }]
```
### Non-class dependencies
Not all dependencies are classes. Sometimes you want to inject a string, function or object.

Apps often define configuration objects with lots of small facts, like the title of the application or the address of a web API endpoint. These configuration objects aren't always instances of a class. They can be object literals, as shown in the following example.
```javascript
export const HERO_DI_CONFIG: AppConfig = {
  apiEndpoint: 'api.heroes.com',
  title: 'Dependency Injection'
};
```
> Typescript interfaces are not valid tokens.

The HERO_DI_CONFIG constant conforms to the AppConfig interface. Unfortunately, you cannot use A Typescript interfae as a token. In Typescript, an interface is a design-time artifact and doesn't have a runtime representation (token) that the DI framework can use.

```javascript
// FAIL! Can't use interface as provider token
[{ provide: AppConfig, useValue: HERO_DI_CONFIG })]

// FAIL! Can't inject using the interface as the parameter type
constructor(private config: AppConfig){ }
```
> This might seem strange if you're used to dependency injection in stronglt typed language where an interface is the preferred depedency lookup key. However Javascript, doesn't have interfaces, so when Typescript is transpiled to Javascript, the interface disappears. There is not interface type information left for Angular to find at runtime.

One alternative is to provide and inject the configuration object in an NgModule like AppModule.
```javascript
providers: [
  UserService,
  { provide: APP_CONFIG, useValue: HERO_DI_CONFIG }
],
```

Another solution to choosing a provider token for non-class dependencies is to define and use an InjectionToken object. The following example how to define such a token.
```javascript
import { InjectionToken } from '@angular/core';

export const APP_CONFIG = new InjectionToken<AppConfig>('app.config');
```
The type parameter, while optional, conveys the dependency type to developers and tooling. The token description is another developer aid.

Register the dependency provider using the InjectionToken object:
```javascript
providers: [{ provide: APP_CONFIG, useValue: HERO_DI_CONFIG }]
```

Now you can inject the configuration object into any constructor that needs it, with the help of an @Inject() parameter decorator.
```javascript
constructor(@Inject(APP_CONFIG) config: AppConfig) {
  this.title = config.title;
}
```

## Factory providers
Sometimes you need to create a dependent value dynamically, based on information you won't have until run time. For example, you might need information that changes repeatedly in the course of the browser session. Also, you injectable service might not have independent access to the source of the information.

In case like this you can use a factory provider. Factory providers can also be useful when creating an instance of a dependency from a third-party that wans't designed to work with DI.

For example, suppose HeroService must hide secret heroes from normal users. Only authorized users should see secret heroes.

Like EventBetterLogger, HeroService needs to know if the user is authorized to see secret heroes. That authorization can-change during course of a single application session, a when you log in a different user.

Imagine that you don't want to inject UserService directly into HeroService, because you don't want to complicate that service with security-sensitive information. HeroService won't have direct access to the user information to decide who is authorized and who isn't.

To resolve this give the HeroService constructor a boolean flag to control display of secret heroes.
```javascript
constructor(
  private logger: Logger,
  private isAuthorized: boolean) { }

getHeroes() {
  const auth = this.isAuthorized ? 'authorized ' : 'unauthorized';
  this.logger.log(`Getting heroes for ${auth} user.`);
  return HEROES.filter(hero => this.isAuthorized || !hero.isSecret);
}
```
You can inject Logger, but you can't inject the isAuthorized flag. Instead, you can use a factory provider to create a new logger instance for HeroService.

A factory provider needs a factory function.
```javascript
const heroServiceFactory = (logger: Logger, userService: UserService) => {
  return new HeroService(logger, userService.user.isAuthorized);
};
```
Although HeroService has no access to UserService, the factory function does. You inject both Logger and UserService into the factory provider and let the injector pass them along to the factory function.
```javascript
export let heroServiceProvider =
  { provide: HeroService,
    useFactory: heroServiceFactory,
    deps: [Logger, UserService]
  };
```
- The useFactory field tells Angular that the provider is a factory function whose implementation is heroServiceFactory.
- The deps property is an array of provider tokens. The Logger and UserService classes serve as token for their own class providers. The injector resolves these tokens and injects the corresponding services into the matching factory function parameters.

Notice that you captured the factory provider in an exported variable. heroServiceProvider. This extra step makes the factory provider resuable. You can configure a provider of HeroService with this variable wherever you need it. In this sample, you need it only in HeroesComponent, where heroServiceProvider replaces HeroService in the metadata providers array.

The following shows the new and the old implementation side-by-side.
```javascript
//heroes.component.ts (v3)
import { Component } from '@angular/core';
import { heroServiceProvider } from './hero.service.provider';

@Component({
  selector: 'app-heroes',
  providers: [ heroServiceProvider ],
  template: `
    <h2>Heroes</h2>
    <app-hero-list></app-hero-list>
  `
})
export class HeroesComponent { }

//heroes.component.ts (v2)
import { Component } from '@angular/core';

import { HeroService } from './hero.service';

@Component({
  selector: 'app-heroes',
  providers: [ HeroService ],
  template: `
    <h2>Heroes</h2>
    <app-hero-list></app-hero-list>
  `
})
export class HeroesComponent { }
```

### Predefined token and multiple providers
Angular provides a number of built-in injection-token constants that you can use to customize the behavior of various systems.

For example, you can use the following built-in tokens as hooks into the frameworks bootstrapping and initialization process. A provider object can associate any of these injection tokens with one or more callback functions that take app-specific initialization actions.
- **PLATFORM_INITIALIZER**: Callback is invoked when platform is initialized.
- **APP_BOBTSTRAP_LISTENER**": Callback is invoked for each component that is bootstrapped. The handler function receivies the ComponentRef instancd of the bootstrapped component.
- **APP_INITIALZER**: Callback is invoked before an app is intialized. All registered initializers can optionally return a Promise. All initializer function that return Promises must be resolved before the application is bootstrapped. If one of the intializers fail to resolves, the application is not bootstrapped.

The provider object can have a third option `multi: true`, which you use with APP_INITIALIZER to register multiple handlers for the provide event.

For example, when bootstrapping an application, you can register many intializers using the same token.
```javascript
export const APP_TOKENS = [
 { provide: PLATFORM_INITIALIZER, useFactory: platformInitialized, multi: true },
 { provide: APP_INITIALIZER, useFactory: delayBootstrapping, multi: true },
 { provide: APP_BOOTSTRAP_LISTENER, useFactory: appBootstrapped, multi: true },
];
```
Multiple providers can be associated with a single token in other areas as well. For example, you can register a custom form validator using the built-in `NG_VALIDATORS` token and provide multiple instances of a given validator provider by using the `multi: true` property in the provider object. Angular add your custom validators to the existing collection.

The Router also makes use of multiple providers associated with a single token. When you provide multiple set of routes using `RouterModule.forRoot` and `RouterModule.forChild` in as single module, the `ROUTES` token combine all the different provided sets of routes into a single value.

You can use ReadonlyArray to type of your multi provider, so Typescript triggers an error in case of unwanted array mutations.
```javascript
constructor(@Inject(MULTI_PROVIDER) multiProvider: ReadonlyArray<MultiProvider>) {
}
```

## Tree-shakable providers
Tree-shaking refers to a compiler option that removes the code from the final bundle if the app doesn't reference that code. When providers are tree-shakable, the Angular compiler removes the associated services from the final output when it determines that your application doesn't use those services. This significantly reduces the size of your bundles.

> Ideally, if an application isn't injecting a service, Angular shouldn't include it in the final output. However, Angular has to be able to identify at build time whether the app will require the service or not. Because it's always possible to inject a service directly using `injector.get(Service)`, Angular can't identify all of the place in your code where this injection could happen, so it has no choice but ot include the service in the injector. Thus, services in the NgModules providers array or at component level are not tree-shakable.

The following example of non-tree-shakable providers in Angular configures a service provider for the injector of an NgModule.
```javascript
import { Injectable, NgModule } from '@angular/core';

@Injectable()
export class Service {
  doSomething(): void {
  }
}

@NgModule({
  providers: [Service],
})
export class ServiceModule {
}
```
You can then import this module into your application to make the servie available for injection in your app, as in the following example.
```javascript
@NgModule({
  imports: [
    BrowserModule,
    RouterModule.forRoot([]),
    ServiceModule,
  ],
})
export class AppModule {
}
```
When ngc runs, it compiles AppModule into a module factory, which contains definitions for all the providers declared in all the modules it includes. At runtime, this factory becomes an injector that instantiates these services.

Tree-shaking doesn't work here because Angular can't decide to exclude one chunk of code(the provider definition for the service within the module factory) based on whether another chunk of code(the service class) is used. To make service tree-shakable the information about how to construct an instance of the service(the provider defination) needs to be a part of the service class itself.

### Creating tree-shakable providers
You can make a provider tree-shakable by specifying it in the @Injectable() decorator on the service itself, rather than in the metadata for the NgModule or component that depends on the service.

The following example shows the tree-shakable equivalent to the ServiceModule example above.
```javascript
@Injectable({
  providedIn: 'root',
})
export class Service {
}
```
The service can be instantiated by configuring a factory function, as in the following example.
```javascript
@Injectable({
  providedIn: 'root',
  useFactory: () => new Service('dependency'),
})
export class Service {
  constructor(private dep: string) {
  }
}
```
> To override a tree-shakable provider, configure the injector of a specific NgModule or component with another provider, using the providers: [] array syntax of the @NgModule() or @Component() decorator.

# Dependency injection in action

## Nested Service Dependencies
The consumer of an injected service doesn't need to know how to create that service. It's the job of the DI framework to create and cache dependencies. The consumer just need to let the DI framework know which dependencies it needs.

Sometimes a service depends on other services, which may depend on yet other service. The dependency injection framework resolves these nested depedencies in the correct order. At each step, the consumer of dependencies declares what is requires in its constructor, and lets the framework provide them.

The following example shows that AppComponent declares its dependence on LoggerService and UserContext.
```javascript
constructor(logger: LoggerService, public userContext: UserContextService) {
  userContext.loadUser(this.userId);
  logger.logInfo('AppComponent initialized');
}
```
UserContext in turn depends on both LoggerService and UserService, another service that gathers information about a particular user.
```javascript
@Injectable({
  providedIn: 'root'
})
export class UserContextService {
  constructor(private userService: UserService, private loggerService: LoggerService) {
  }
}
```
When Angular creates AppComponent, the DI framework creates an instance of LoggerService and starts to create UserContextService. UserContextService also needs LoggerService, which the framework already has, so the framework can provide the same instance. UserContextService also needs UserService, which the framework has yet to create. UserService has no further dependencies, so the framework can simply use new to inistantiate the class and provide the instance to the UserContextService constructor.

The parent AppComponent doesn't need to know about the dependencies of dependencies. Declare what's needed in the constructor(in this case LoggerService and UserContextService) and the framework resolves the nested dependencies.

## Limit service scope to a component subtree
An Angular application has multiple injectors, arranged in a tree hierarchy that parallels the component tree. Each injector creates a singleton instance of a dependency. That same instance id injected wherever that injector provides that service. A particular service can be provided and create at any level of the injector hierarchy, which means that there can be multiple instances of a services if it is provided by multiple injectors.

Dependencies provided by the root injector can be injected into any component anywhere in the application. In some cases, you might want to restrict service availiabilty to a particular region of the application. For instance, you might want to let users explicitly opt in to use a service rather than letting the root injector provide it automatically.

You can limit the scope of an injected service to a branch of the application hierarchy by providing that service at the sub-root component for that branch. This example shows how to make a different instance of HeroService available to HeroBaseComponent by adding it to the providers array of the @Component() decorator of the sub-component.

```javascript
@Component({
  selector: 'app-unsorted-heroes',
  template: `<div *ngFor="let hero of heroes">{{hero.name}}</div>`,
  providers: [HeroService]
})
export class HeroesBaseComponent implements OnInit {
  constructor(private heroService: HeroService) { }
}
```
When Angular creates HeroBaseComponent, it also creates a new instance of HeroService that is visible only to that component and its children, if any

You could also provide HeroService to a different component elsewhere in the application. That would result in a different instance of the service, living in a different injector.

## Multiple service instances (sandboxing)
Sometimes you want multiple instances of a service at the same level of the component hierarchy.

A good example is a service that holds state for its companion component instance. You need a separate instance of the service for each component. Each service has its own work-state, isolated from the service-and-state of a different component. This is called _sandboxing_ because each service and component instance has its own sandbox to play in.

In this example, HeroBiosComponent present three instances of HeroBioComponent.
```javascript
@Component({
  selector: 'app-hero-bios',
  template: `
    <app-hero-bio [heroId]="1"></app-hero-bio>
    <app-hero-bio [heroId]="2"></app-hero-bio>
    <app-hero-bio [heroId]="3"></app-hero-bio>`,
  providers: [HeroService]
})
export class HeroBiosComponent {
}
```

Each HeroBioComponent can edit a single hero biography. HeroBioComponent relies on the HeroCacheService to fetch catche and perform other persistence operations on that hero.
```javascript
@Injectable()
export class HeroCacheService {
  hero: Hero;
  constructor(private heroService: HeroService) {}

  fetchCachedHero(id: number) {
    if (!this.hero) {
      this.hero = this.heroService.getHeroById(id);
    }
    return this.hero;
  }
}
```
Three instances of HeroBioComponent can't share the same instance of HeroCacheService as they'd be competing with each other to determine which hero to cache.

Instead, each HeroBioComponent gets its own HeroCacheService instance by listing HeroCacheService in its metadata providers array.
```javascript
@Component({
  selector: 'app-hero-bio',
  template: `
    <h4>{{hero.name}}</h4>
    <ng-content></ng-content>
    <textarea cols="25" [(ngModel)]="hero.description"></textarea>`,
  providers: [HeroCacheService]
})

export class HeroBioComponent implements OnInit  {
  @Input() heroId: number;

  constructor(private heroCache: HeroCacheService) { }

  ngOnInit() { this.heroCache.fetchCachedHero(this.heroId); }

  get hero() { return this.heroCache.hero; }
}
```

The parent HeroBiosComponent binds a value to heroId. ngOnInit passes that ID to the service, which fetches and caches the hero. The getter for the hero property pulls the cached hero from the service. The template displays this data-bound property.

## Qualify dependency lookup with parameter decorators
When a class required a dependency, that dependency is added to the constructor as a parameter. When Angular needs to instantiate the class, it calls upon DI framework to supply the dependency. By default, the DI framework searches for a provider in the injector hierarchy, starting at the component's local injector of the component, and if necessary bubbling up through the injector tree until it reaches the root injector.
- The first injector configured with a provider supplies the dependency(a service instance or value) to the constructor.
- If no provider is found in the root injector, the DI framework throws an error.

There are a number of options for modifying the default search behavior, using parameter decorators on the service-valued parameters of a class constructor.

### Make a dependency @Optional and limit search with @Host
Dependencies can be registered at any level in the component hierarchy. When a component requests a dependency. Angular starts with that component's injector and walks up the injector tree until it finds the first suitable provider. Angular throws an error if it can't find the dependency during that walk.

In some cases, you need to limit the search or accommodate a missing dependency. You can modify Angular's search behavior with the @Host and @Optional qualifying decorators on a service-valued parameter of the component's constructor.
- The @Optional property decorator tells Angular to return null when it can't find the depedency.
- The @Host property decorator stops the upward search at the host component. The host component is typically the component requesting dependency. However, when this component is projected into a parent component, that parent component becomes the host. The following example covers this second case.

These decorators can be used individually or together as shown in the example. This HeroBiosAndContactsComponent is a revision of HeroBiosComponent.
```javascript
@Component({
  selector: 'app-hero-bios-and-contacts',
  template: `
    <app-hero-bio [heroId]="1"> <app-hero-contact></app-hero-contact> </app-hero-bio>
    <app-hero-bio [heroId]="2"> <app-hero-contact></app-hero-contact> </app-hero-bio>
    <app-hero-bio [heroId]="3"> <app-hero-contact></app-hero-contact> </app-hero-bio>`,
  providers: [HeroService]
})
export class HeroBiosAndContactsComponent {
  constructor(logger: LoggerService) {
    logger.logInfo('Creating HeroBiosAndContactsComponent');
  }
}
```

Focus on the template
```javascript
template: `
  <app-hero-bio [heroId]="1"> <app-hero-contact></app-hero-contact> </app-hero-bio>
  <app-hero-bio [heroId]="2"> <app-hero-contact></app-hero-contact> </app-hero-bio>
  <app-hero-bio [heroId]="3"> <app-hero-contact></app-hero-contact> </app-hero-bio>`,
```
Now there's a new &lt;hero-contact&gt; element between the &lt;hero-bio&gt; tags. Angular projects, or trancludes the corresponding HeroContactComponent into the HeroBioComponent view placing it in the &lt;ng-content&gt; slot of the HeroBioComponent template.
```javascript
template: `
  <h4>{{hero.name}}</h4>
  <ng-content></ng-content>
  <textarea cols="25" [(ngModel)]="hero.description"></textarea>`,
```
Here's HeroContactComponent, which demonstrates the qualifying decorators.
```javascript
@Component({
  selector: 'app-hero-contact',
  template: `
  <div>Phone #: {{phoneNumber}}
  <span *ngIf="hasLogger">!!!</span></div>`
})
export class HeroContactComponent {

  hasLogger = false;

  constructor(
      @Host() // limit to the host component's instance of the HeroCacheService
      private heroCache: HeroCacheService,

      @Host()     // limit search for logger; hides the application-wide logger
      @Optional() // ok if the logger doesn't exist
      private loggerService?: LoggerService
  ) {
    if (loggerService) {
      this.hasLogger = true;
      loggerService.logInfo('HeroContactComponent can log!');
    }
  }

  get phoneNumber() { return this.heroCache.hero.phone; }

}
```
Focus on the constructor parameters.
```javascript
@Host() // limit to the host component's instance of the HeroCacheService
private heroCache: HeroCacheService,

@Host()     // limit search for logger; hides the application-wide logger
@Optional() // ok if the logger doesn't exist
private loggerService?: LoggerService
```

The @Host() function decorating the heroCache constructor property ensures that you get a reference to the cache service from the parent HeroBioComponent. Angular throws an error if the parent lacks that service, even if a component higher in the component tree includes it.

A second @Host() function decorates the loggerService constructor property. The only LoggerService instance in the app is provided at the AppComponent level. The host HeroBioComponent doesn't have its own LoggerService provider.

Angular throws an error if you haven't also decorated the property with @Optional(). When the property is marked as optional, Angular sets loggerService to null and the rest of the component adapts.

If you comment out the @Host() decorator, Angular walk up the injector ancestor tree until it finds the logger at the AppComponent level. The logger logic kicks in and the hero display updates with the "!!!" marker to indicate that the logger was found.

If you restore the @Host() decorator and comment out @Optional, the app throw an exception when it cannot find the required logger at the host component level.

> EXCEPTION: No provider for LoggerService(HeroContactComponent -> LoggerService)

### Supply a custom provider with @Inject
Using a custom provider allows you to provide a concrete implelementation for implicit depedendencies, such as built-in-browser API. The following example use an InjectionToken to provider the localStorage browser API as a dependency in the BrowserStorageService.
```javascript
import { Inject, Injectable, InjectionToken } from '@angular/core';

export const BROWSER_STORAGE = new InjectionToken<Storage>('Browser Storage', {
  providedIn: 'root',
  factory: () => localStorage
});

@Injectable({
  providedIn: 'root'
})
export class BrowserStorageService {
  constructor(@Inject(BROWSER_STORAGE) public storage: Storage) {}

  get(key: string) {
    this.storage.getItem(key);
  }

  set(key: string, value: string) {
    this.storage.setItem(key, value);
  }

  remove(key: string) {
    this.storage.removeItem(key);
  }

  clear() {
    this.storage.clear();
  }
}
```

The factory function returns the localStorage property that it attached to the browser window object. The Inject decorator is a constructor parameter used to specify a custom of a dependency. This custom provider can now be overridden during testing with a mock API of localStorage instead of interacting with real browser APIs.

### Modify the provider search with @Self and @SkipSelf
Providers can also be scoped by injector through constructor parameters decorators. The following example overrides the BROWSER_STORAGE token in the Component class provider with the sessionStorage browser API. The same BrowserStorageService is injected twice in the constructor, decorated with @Self and @SkikSelf to define which injector handles the provider dependency.
```javascript
import { Component, OnInit, Self, SkipSelf } from '@angular/core';
import { BROWSER_STORAGE, BrowserStorageService } from './storage.service';

@Component({
  selector: 'app-storage',
  template: `
    Open the inspector to see the local/session storage keys:

    <h3>Session Storage</h3>
    <button (click)="setSession()">Set Session Storage</button>

    <h3>Local Storage</h3>
    <button (click)="setLocal()">Set Local Storage</button>
  `,
  providers: [
    BrowserStorageService,
    { provide: BROWSER_STORAGE, useFactory: () => sessionStorage }
  ]
})
export class StorageComponent implements OnInit {

  constructor(
    @Self() private sessionStorageService: BrowserStorageService,
    @SkipSelf() private localStorageService: BrowserStorageService,
  ) { }

  ngOnInit() {
  }

  setSession() {
    this.sessionStorageService.set('hero', 'Dr Nice - Session');
  }

  setLocal() {
    this.localStorageService.set('hero', 'Dr Nice - Local');
  }
}
```
Using the @Self decorator, the injector only looks at the component's injector for its providers. The @SkipSelf decorator allows you to skip the local injector and look up in the hierarchy to find a provider that satisfies this dependency. The sessionStorageService instance interacts with the BrowserStorageService using the sessionStorage browser API, while the localStorageService skips the local injector and uses the root BrowserStorageService that uses the localStorage browser API.

## Inejct the component's DOM element
Although developers strive to avoid it, many visual effects and third-party tools, such as jQuery, require DOM access. As a result, you might need to access a component's DOM element.

To illustrate, here simplified version of HighlightDirective from the Attribute Directives page.
```javascript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {

  @Input('appHighlight') highlightColor: string;

  private el: HTMLElement;

  constructor(el: ElementRef) {
    this.el = el.nativeElement;
  }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor || 'cyan');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.style.backgroundColor = color;
  }
}
```
The directive sets the background to a highlight color when the user mouses over the DOM element to which the directive is applied.

Angular sets the constructor's el parameter to the injected ElementRef. (An ElementRef is a wrapper around DOM element, whose nativeElement property exposes the DOM element for the directive to manipulate.)

The sample code applies the directive's myHightlight attribute to two &lt;div&gt;, first without a value(yielding the default color) and then with an assigned color value.

```html
<div id="highlight"  class="di-component"  appHighlight>
  <h3>Hero Bios and Contacts</h3>
  <div appHighlight="yellow">
    <app-hero-bios-and-contacts></app-hero-bios-and-contacts>
  </div>
</div>
```

## Define dependencies with providers
In order to get a service from dependency injector, you have to give it a token. Angular usually handles this transaction by specifying a constructor parameter and its type. The parameter type serves as the injector lookup token. Angular passes this token to the injector and assigns the result to the parameter.

The following is a typical example.
```javascript
constructor(logger: LoggerService) {
  logger.logInfo('Creating HeroBiosComponent');
}
```
Angular asks the injector for the service associated with LoggerService and assigns the returned values to the logger parameter.

If the injector has already cached an instance of the service associated with the token, it provides that instances. If it doesn't, it need to make one using the provider associated with the token.

> If the injector doesn't have a provider for a requested token, it delegates the request to its parent injector, where the process repeats until there are no more injectors. If the search fails, the injector throws an error - unless the request was optional.

A new injector has no providers. Angular initializes the injector it creates with a set of preferred providers. You have to configure providers for your own app-specific dependencies.

### Defining providers
A dependency can't always be created by the default method of instantiating a class. You learned about some other methods in Depenedency providers. The following HeroOfTheMonthCompoment example demonstrates many of the alternatives and why you need them. It's visually simple: a few properties and the logs produced by a logger.
```javascript
import { Component, Inject } from '@angular/core';

import { DateLoggerService } from './date-logger.service';
import { Hero } from './hero';
import { HeroService } from './hero.service';
import { LoggerService } from './logger.service';
import { MinimalLogger } from './minimal-logger.service';
import { RUNNERS_UP,
         runnersUpFactory } from './runners-up';

@Component({
  selector: 'app-hero-of-the-month',
  templateUrl: './hero-of-the-month.component.html',
  providers: [
    { provide: Hero,          useValue:    someHero },
    { provide: TITLE,         useValue:   'Hero of the Month' },
    { provide: HeroService,   useClass:    HeroService },
    { provide: LoggerService, useClass:    DateLoggerService },
    { provide: MinimalLogger, useExisting: LoggerService },
    { provide: RUNNERS_UP,    useFactory:  runnersUpFactory(2), deps: [Hero, HeroService] }
  ]
})
export class HeroOfTheMonthComponent {
  logs: string[] = [];

  constructor(
      logger: MinimalLogger,
      public heroOfTheMonth: Hero,
      @Inject(RUNNERS_UP) public runnersUp: string,
      @Inject(TITLE) public title: string)
  {
    this.logs = logger.logs;
    logger.logInfo('starting up');
  }
}
```
The providers array show how you might use the different provider-definition keys; useValue, useClass, useExisting or useFactory.

### Value providers: useValue
The userValue key lets you associated a fixed value with a DI token. Use this technique to provide runtime configuration constants such as website base addresses and feature flag. You can also use a value provider in a unit test to provide mock data in place of a production data service.

The HeroOfTheMonthComponent example has two value providers
```javascript
{ provide: Hero,          useValue:    someHero },
{ provide: TITLE,         useValue:   'Hero of the Month' },
```
- The first provides an existing instance of the Hero class to use for the Hero token, rather requiring the injector to create a new instance with new or use its own cached instace. Here, the token is the class itself.
- The second specifies a literal string resources to use for the TITLE token. The TTILE provider token is not a class, but is instead a special kind of provider lookup key called an injection token represented by an InjectionToken interface.

You can use an injection token for any kind of provider but it's particularly helpful when the depedency is a simple value like a string, a number, or a function.

The value of a value provider must be defined before you specify it here. The title string literal is immediately available. The someHero variable in this example was set earilier in the file. You can't use a variable whose value will be defined later.
```javascript
const someHero = new Hero(42, 'Magma', 'Had a great month!', '555-555-5555');
```
Other types of providers can create their values lazily, that is, when they're needed for injection.

### Class providers: useClass
The useClass provider key you create and return a new instance of the specified class.

You can use this type of provider to substitute an alternative implementation for a common or default class. The alternative implementation could, for example, implement a different strategy, extends the default class, or emulate the behavior of the real class in a test case.

The following code shows two examples in HeroOfTheMonthComponent
```javascript
{ provide: HeroService,   useClass:    HeroService },
{ provide: LoggerService, useClass:    DateLoggerService },
```
The first provider is the de-sugared, expanded form of the most typical case in which the class to created(HeroService) is also the provider's dependency inkector token. The short form is generally preferred; this long form makes the default explicit.

The second provider substitutes DataLoggerService for LoggerService. LoggerService is already registered at the AppComponent level. When this child component requests LoggerService. It receives a DataLoggerService instance instead.

> This component and its tree of child components receive DataLoggerService instance. Components outside the three continue to receive the original LoggerService instance.

DataLoggerService inherits from LoggerService; it appends the current data/time to each message:
```javascript
@Injectable({
  providedIn: 'root'
})
export class DateLoggerService extends LoggerService
{
  logInfo(msg: any)  { super.logInfo(stamp(msg)); }
  logDebug(msg: any) { super.logInfo(stamp(msg)); }
  logError(msg: any) { super.logError(stamp(msg)); }
}

function stamp(msg: any) { return msg + ' at ' + new Date(); }
```
### Alias providers: useExisting
The useExisting provider key lets you map one token to another. In effect, the first token is an alias for the service associated with the second token, creating two ways to access the same service object.

```javascript
{ provide: MinimalLogger, useExisting: LoggerService },
```
You can use this technique to narrow an API through an aliasing interface. The following example shows an alias introduced for the purpose.

Imagine that LoggerService had a large API, much large than the actual three methods and a property. You might want to shrink that API surface to just the members you actually need. In this example MinimalLogger class-interface reduces the API to two members:

```javascript
// Class used as a "narrowing" interface that exposes a minimal logger
// Other members of the actual implementation are invisible
export abstract class MinimalLogger {
  logs: string[];
  logInfo: (msg: string) => void;
}
```
The following example puts MinimalLogger to use in a simplified version of HeroOfTheMonthComponent.

```javascript
@Component({
  selector: 'app-hero-of-the-month',
  templateUrl: './hero-of-the-month.component.html',
  // TODO: move this aliasing, `useExisting` provider to the AppModule
  providers: [{ provide: MinimalLogger, useExisting: LoggerService }]
})
export class HeroOfTheMonthComponent {
  logs: string[] = [];
  constructor(logger: MinimalLogger) {
    logger.logInfo('starting up');
  }
}
```
The HeroOfTheMonthComponent constructor's logger parameter is typed as MinimalLogger, so only the logs and logInfo members are visible in a Typescript-aware editor.

Behind the scenes, Angular sets the logger parameter to the full service registered under the LoggingService token, which happens to be the DateLoggerService instance that was provided above.

### Factory providers: useFactory
The useFactory provider key lets you create a dependency object by calling a factory function, as in the following example
```javascript
{ provide: RUNNERS_UP,    useFactory:  runnersUpFactory(2), deps: [Hero, HeroService] }
```
The injector provides the dependency value by invoking a factory function, that you provide as the value of the useFactory key. Notice that this form of provider has a third key, deps, which specifies dependencies for the useFactory function.

Use this technique to create a dependency object with a factory function whose inputs are a combination of injected services and local state.

The dependency object(returned by the factory function) is typically a class instance, but can be other things as well. In this example, the dependency object is a string of the names of the runners up to the "Hero of the Month" contest.

In the example, the local state is the number 2, the number of runners up the component should show. The state value is passed as an argument to runnersUpFactory(). The runnersUpFactory() returns the provider factory function, which can use both the passed-in state value and the injected services Hero and HeroService.

```javascript
export function runnersUpFactory(take: number) {
  return (winner: Hero, heroService: HeroService): string => {
    /* ... */
  };
}
```
The provider factory function(returned by runnersUpFactory()) returns the actual dependency object the string of names.
- The function takes a winning Hero and a HeroService as arguments. Angular supplies these arguments from injected values identified by the two tokens in the deps array.
- The function returns the string of names, which Angular than injects into the runnersUp parameter of HeroOfTheMonthComponent.

## Provider token alternatives: class interface and InjectionToken
Angular dependency injection is easiest when the provider token is a class that is also the type of the returned dependency object, or service.

However, a token doesn't have to be a class and even when it is class, it doesn't to be the same type as the returned object. 
### Class interface
The previous Hero of the Month example used the MinimalLogger class as the token for a provider of LoggerService.
```javascript
{ provide: MinimalLogger, useExisting: LoggerService },
```
MinimalLogger is an abstract class
```javascript
// Class used as a "narrowing" interface that exposes a minimal logger
// Other members of the actual implementation are invisible
export abstract class MinimalLogger {
  logs: string[];
  logInfo: (msg: string) => void;
}
```
An abstract class is usually a base class that you can extend. In this app, however there is no class that inherits from MinimalLogger, or they could have implemented it instead, in the manner of an interface. But they did neither. MinimalLogger is used only as dependency injection token.

When you use a class this way, it's called a _class interface_.

As mentioned as DI providers, an interface is not a valid DI token because it is a Typescript artificat that doesn't exist at run time. Use this abstract class interface to get the string typing of an interface, and also use it as a provider token in the way you would a normal class.

A class interface should define only the members that its consumers are allowed to call. Such a narrowing interface helps decouple the concrete class form its consumers.

### InjectionToken objects
Dependency objets can be simple values like data, numbers and strings or shapeless objects like arrays and functions.

Such objects don't have application interfaces and therefore aren't well represented by a class. They're better represented by a token that is both unique and symbolic a Javascript object that has a friendly name but won't conflict with another token that happend to have the same name.

InjectionToken has these characteristics. You encountered them twice in the Hero of the Month example, in the title value provider and in the runnersUp factory provider.
```javascript
{ provide: TITLE,         useValue:   'Hero of the Month' },
{ provide: RUNNERS_UP,    useFactory:  runnersUpFactory(2), deps: [Hero, HeroService] }
```
You create the TITLE token like this:
```javascript
import { InjectionToken } from '@angular/core';

export const TITLE = new InjectionToken<string>('title');
```

The type parameter, while optional conveys the dependency's type to developers and tooling. The token description is another developer aid.

## Inject into a derived class
Take care when writing a component that inherits from another component. If the base component has injected dependencies, you must re-provide and re-inject them in the derviced class and then pass then down to the base class through the constructor.

In this contrived example, SortedHeroesComponent inherits from HeroBaseComponent to display a sorted list of heroes.

The HeroBaseComponent can stand on its own. It demands its own instance of HeroService to get heroes and displays them in the order they arrive from the database.

```javascript
@Component({
  selector: 'app-unsorted-heroes',
  template: `<div *ngFor="let hero of heroes">{{hero.name}}</div>`,
  providers: [HeroService]
})
export class HeroesBaseComponent implements OnInit {
  constructor(private heroService: HeroService) { }

  heroes: Array<Hero>;

  ngOnInit() {
    this.heroes = this.heroService.getAllHeroes();
    this.afterGetHeroes();
  }

  // Post-process heroes in derived class override.
  protected afterGetHeroes() {}

}
```
Users wants to see the heroes in alphabetical order. Rather than modify the original component, sub-class it and create a SortHeroesComponent that sorts heroes before presenting them. The SortedHeroesComponent lets the base class fetch the heroes.

Unfortunately, Angular cannot inject the HeroService directly into the base class. You must provide the HeroService again for this component, then pass it down to the base class inside the constructor.

```javascript
@Component({
  selector: 'app-sorted-heroes',
  template: `<div *ngFor="let hero of heroes">{{hero.name}}</div>`,
  providers: [HeroService]
})
export class SortedHeroesComponent extends HeroesBaseComponent {
  constructor(heroService: HeroService) {
    super(heroService);
  }

  protected afterGetHeroes() {
    this.heroes = this.heroes.sort((h1, h2) => {
      return h1.name < h2.name ? -1 :
            (h1.name > h2.name ? 1 : 0);
    });
  }
}
```
Now take note of the afterGetHeroes() method. Your first instinct might have been to create an ngOnInit method in SortedHeroesComponent and do the sorting there. But Angular calls the dervied class's ngOnInit before calling the base class's ngOnInit so you'd be sorting the heroes array before they arrived. That produces a nasty error.

Overriding the base class's afterGetHeroes() method solves the problem.

These complications argue for avoiding component inheritance.

## Break circularities with a forward class reference (forwardRef)
The order of class declaration matters in Typescript. You can't refer directly to a class until it's been defined. 

This isn't usually a problem, especially if you adhere to the recommended one class per file rule. But sometimes circular references are unavoidable. You're in a bind when class 'A' refers to class 'B' and 'B' referes to 'A'. One of them has to be defined first.

The Angular forwardRef() function creates an indirect reference that Angular can resolve later.

The Parent Finder sample is full of circular class reference that are impossible to break.

You face this dilemma when a class makes a reference to itself as does AlexComponent in its providers array. The providers array is a property of the @Component() decorator function which must appear above the class definition.

Break the circularity with forwardRef.
```javacript
providers: [{ provide: Parent, useExisting: forwardRef(() => AlexComponent) }],
```

## Navigate the component tree with DI
Application components often need to share information. You can often use loosely coupled techniques for sharing information, such as data binding and service sharing, but sometimes it makes sense for one component to have a direct reference to another component. You need a direct reference, for instance, to access values or call methods on that component.

Obtaining a component reference is a bit tricky in Angular. Angular components themselves do not have a tree that you can inspect or navigate programmatically. The parent-child relationship is indirect, established through the component's view objects.

Each component has a host view, and can have additional embedded views. An embedded view in component A is the host view of component B, which can in turn have embedded view. This means that thereis a view hierarchy for each component, of which that component's host view is the root.

There is an API for navigating down the view hierarchy. Check out Query, QueryList, ViewChildren and ContentChildren.

There is no public API for acquiring a parent reference. However, because every component instance is added to an injector's container, you can use Angular dependency injection to reach a parent component.

### Find  a parent component of known type
You use standard class injection to acquire a parent component whose type you know.

In the following example, the parent AlexComponent has several children including a CathyComponent:
```javascript
@Component({
  selector: 'alex',
  template: `
    <div class="a">
      <h3>{{name}}</h3>
      <cathy></cathy>
      <craig></craig>
      <carol></carol>
    </div>`,
})
export class AlexComponent extends Base
{
  name = 'Alex';
}
```
Cathy reports whether or not she has access to Alex after injecting an AlexComponent into her constructor.
```javascript
@Component({
  selector: 'cathy',
  template: `
  <div class="c">
    <h3>Cathy</h3>
    {{alex ? 'Found' : 'Did not find'}} Alex via the component class.<br>
  </div>`
})
export class CathyComponent {
  constructor( @Optional() public alex?: AlexComponent ) { }
}
```

### Unable to find a parent by its base class
What if you don't know the concrete parent component class?

A re-usable component might be a child of multiple components. Imaging a component for rendering breaking news about a financial instrument. For business reasons, this news component make frequenct call directly into its parent instrument as changing market data streams by.

The app probably defines more than a dozen financial instrutment components. If you're lucky, they all implement the same base class whose API your NewComponent understand.

> Looking for components that implement an interface would be better. That's not possible because Typescript interfaces disappear from the transplied Javascript which doesn't support interfaces. There's no artifact to look for.

This isn't necessarily good design. This example is examining whether a component can inject its parent vai the parent's base class.

The Alex component extends (inherits) from a class named Base.

```javascript
export class AlexComponent extends Base
```

The CraigComponent tries to inject Base int its alex constructor parameter and reports if it succeeded.
```javascript
@Component({
  selector: 'craig',
  template: `
  <div class="c">
    <h3>Craig</h3>
    {{alex ? 'Found' : 'Did not find'}} Alex via the base class.
  </div>`
})
export class CraigComponent {
  constructor( @Optional() public alex?: Base ) { }
}
```
### Find a parent by its class interface
You can find a parent component with a class interface

The parent must cooperate by providing an alias to itself in the name of a class interface token.

Recall the Angular always adds a component instance to its own injector; that's why you could inject Alex into Cathy eariler.

Write an alias provider - a provider object literal with a useExisting definition - that creates an alternative way to inject the same component instance and add the provider to the providers array of the @Component() metadata for the AlexComponent.
```javascript
providers: [{ provide: Parent, useExisting: forwardRef(() => AlexComponent) }],
```
Parent is the provider's class interface token. The forwardRef breaks the circular reference you just created by having the AlexComponent refer to itself.

Carol, the third of Alex's child component injects the parent into its parent parameter, the same wat you've done it before.
```javascript
export class CarolComponent {
  name = 'Carol';
  constructor( @Optional() public parent?: Parent ) { }
}
```
### Find a parent in a tree with @SkipSelf() 
Imagine one branch of a component hierarchy: Alice -> Barry -> Carol. Both Alice and Barry implements the Parent class interface.

Barry is the problem. Her needs to reach his parent, Alice and also be a parent to Carol. That means he must bsth inject the Parent class interface to get Alice and provide a Parent to satisfy Carol.

Here's Barry
```javascript
const templateB = `
  <div class="b">
    <div>
      <h3>{{name}}</h3>
      <p>My parent is {{parent?.name}}</p>
    </div>
    <carol></carol>
    <chris></chris>
  </div>`;

@Component({
  selector:   'barry',
  template:   templateB,
  providers:  [{ provide: Parent, useExisting: forwardRef(() => BarryComponent) }]
})
export class BarryComponent implements Parent {
  name = 'Barry';
  constructor( @SkipSelf() @Optional() public parent?: Parent ) { }
}
```
Barry's providers array look just like Alex's. If you're going to keep writing alias providers like this you should create a helper function.

For now, focus on Barry's constructor
```javascript
//Barry's Constructor
constructor( @SkipSelf() @Optional() public parent?: Parent ) { }
//Carol's Constructor
constructor( @Optional() public parent?: Parent ) { }
```
It's identical to Carol constructor expcept for the additional @SkipSelf deocrator.
@SkipSelf is essential for two reasons:
1. It tells the injector to start for a Parent dependency in a component above itself, which is what parent means.
2. Angular throws a cyclic dependency error if you omit the @SkipSelf decorator. `Circular depependecy in DI detected for BethCompoent. Dependency path: BethComponent->Parent -> BethComponent.`

### Parent class interface
A Class interface is an abstract class used as an interface rather than as a base class.

The example defines a Parent class interface
```javascript
export abstract class Parent { name: string; }
```
The parent class interface defines a name property with a type declaration but no implementation. The name property is the only member of a parent component that a child component can call. Such a narrow interface helps decouple the child component class from its parent components.

A component that could serve as a parent should implement the class interface as the AliceComponent does.

```javascript
export class AliceComponent implements Parent
```
Doing so adds clarity to the code. But it's not technically necessary. Although AlexComponent has a name property, as required by its Base class its, class signature doesn't mention Parent.

```javascript
export class AlexComponent extends Base
```

> AlexComponent should implement Parent as a matter of proper style. It doesn't in this example only to demonstrate that the code will compile and run without the interface.

### provideParent() helper function
Writing variations of the same parent alias provider gets old quickly, especially this awful mouthful with a forwardRef.
```javascript
providers: [{ provide: Parent, useExisting: forwardRef(() => AlexComponent) }],
```
You can extract that logic into a helper function like the following
```javascript
// Helper method to provide the current component instance in the name of a `parentType`.
export function provideParent
  (component: any) {
    return { provide: Parent, useExisting: forwardRef(() => component) };
  }
```  
Now you can add a simpler, more meaningful parent provider to your components
```javascript
providers:  [ provideParent(AliceComponent) ]
```

You can do better. The current version oof the helper function can only alias the Parent class interface. The application might have a variety of parent types, each with its own class interface token.

Here's a revised version that defaults to parent but alis accepts an optional second parameter for a different parent class interface.
```javascript
// Helper method to provide the current component instance in the name of a `parentType`.
// The `parentType` defaults to `Parent` when omitting the second parameter.
export function provideParent
  (component: any, parentType?: any) {
    return { provide: parentType || Parent, useExisting: forwardRef(() => component) };
  }
```
And here's how you could use it with a different parent type.
```javascript
providers:  [ provideParent(BethComponent, DifferentParent) ]
```

Source: [Angular.io](https://angular.io/guide/dependency-injection)