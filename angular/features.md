# Angular 2 
Let us now understand the concerns that Angular2 wanted to address. First off, Angular 2 is high performance - it minimizes the load time of your pages. This section lists the changes that Angular 2 has brought in over it earilier counterparts.
- **TypeScript**: Angular 2 uses Typescript heavily. Incidently, Typescript is a statically typed language like C# and is essentailly a superset of Javascript. This is one of the most striking changes in Angular 2 if you compare this with earilier versions. Typescript has gained a lot of popularity primarly because of its simplicity and ease of use. As Typescript is from Microsoft, Angular 2 has become widely popular amongst the Microsoft community as well. For your information, frameworks like ReactJS also uses Typescript.
- **Mobile development**: Regardless of the fact that Angular 1.x was meant for responsive application development, there wasn't any mobile support. However, you could take advantage of some libraries to run Angular 1.x on mobile. On other hand Angular 2 has built-in support for mobile application development. Note that Angular 2 renders code differently for Web browsers and mobile browsers.
- **Performance**: Performance in Angular 2 is much improved - thanks to the support for offline compilation and fast change detection. The support for dynamic loading and asynchronous templating are features that help in improving the page load and response time considerably. Module loader is also much simplified in Angular 2. You can now use System.js as the universal loader to load the ES6 modules.
- **Directives**: Angular 2 provides support for three types of directives. These are component directives, decorator directives and template directives.
- **Improved Dependency Injection**: Dependency Injection is a software design principle that helps to abstract the dependencies of an object outside of it and make such object loosely coupled with each other. Angular 2 has improved support for dependency injection. Modular development and component isolation are features that make dependency injection simple and easy to use and implement in Angular 2.
- **Improved data binding**: Data binding in Angular 2 is much improved. To bind data to a DOM element, all you have to do is just wrap the element inside a square bracket as shown in the code snippet below:
```html
<img [src]="test.gif">
``` 
- **Support for a component-based architecture**: Unlike its eariler versions, Angular 2 is completely component based. With Angular 2, you would just use component and directives - controllers and $scope are no longer used well.
- **Cross-patform**: You can build and run Angular 2 applications on desktop, Android and iOS systems.
- **Browser Support**: Angular 2 supports most of the modern-day browsers. These include: IE 9, 10, 11, Firefox, Chrome, Safari, Android 4.1 and Microsoft Edge.
- **Improved routing**: Routing in Angular 2 is much improved - thanks to URL resolver, location service, navigational model, etc.


# Angular 4
Roughly six month after the release of Angular 2, the next big update for Angular is now available: Angular 4, or rather Angular v4, because the team decided it should be called "just Angular" from now on, without stating the version number explicitly in the name. Originally, the "2" was used to differentiate between AngularJS and the all new Angular Framework, which came with many reassessed and refine concepts. The result, Angular, can be used in many different programming Languages, like Dart, Typescript or ECMAScript 5 among others.

The reason for this new major release are both new features as well as changes incompatible with the previous version. Let's take a look at the new features:
- **Router ParamMap** : Starting from version 4, it is possible to query a so-called ParamMap in the router, meaning a request for the route- and queryparameter assigned to a route.
Up until now, route parameters were stored in a simple key-value object structure, therefore being accessible by using the standard Javascript syntax (parameterObjekt[‘parameter-name’] )
- **Animations**: Functions necessary for animations up until now were provided as part of `@angular/core` module, implying that these parts of the code were always included in applications, even if they did not get used in apps without animations. To avoid creating bundles with unnecessary large sizes, this function has been put into its own package. 
Animations are provided to be in the module BrowserAnimationsModule from `@angular/platform-browser/animations`.
- **ngIf: Can also be used with "else"**: It's quite a frequent thing to use "conditional rendering" in templates to display information dependening on some condition. This is done by using *ngIf. If a condition isn't met the corresponding element and all child element are not added to the DOM tree. Many times there was also a need for the opposing case, making it necessary to formulate the same condition just the other way around and another *ngIf.
This has some nasty implications for readability and maintainability of the code - after all you have to work on multiple lines of code when immplementing some changes.
In Angular 4, this use can be solved with a newly added else. May be unexpected for some, Angular uses a separately references template fragement, which in the else-case will be used in place of the element marked with *ngIf.
- **Dynamic Components with NgComponentOutlet**: The new *ngComponentOutlet-Directive makes it possible to build dynamic components in a declarative way. Up until now, it has been quite a lot of work to build
High cohesion is when you have a class that does a well defined job. Low cohesion is when a class does a lot of jobs that don't have much in common.
Let's take this example:
You have a class that adds two numbers, but the same class creates a window displaying the result. This a low cohesive class because the window  and adding opreation don't have much in common. The window is the visual part of the program and the adding function is the logical behind it.
To create a high cohesive solution, you would have to create a class Window and a class Sum. The window will call Sum's method to get the result and display it. This way you will develop separately the logic and the GUI of your application.

