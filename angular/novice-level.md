# What are the differences between AngularJS (angular 1.x) and Angular (Angular 2.x and beyond)?
- Angular 1.x was not built with mobile support in mind, where Angular 2 is mobile oriented.
- Angular 2 uses Typescript which is a superset of Javascript(It doesn't mean only Typescript but dart also)
- https://angular.io/guide/ajs-quick-reference

# What is a component?
A *component controls* a patch of screen called a *view*. For example, individual component define and control each of the following views:
- The app root with the navigation links
- The list of heroes
- The hero editor

You define a component's application logic - what is does to support the view - inside a class. The class interacts with the view through an API of properties and methods.

For example, *HeroListComponent* has a *heroes* property that holds an array of heroes. Its *selectHero()* method sets a *selectedHero* property when the user clicks to choose a hero from that list. The component accquires the heroes from a service, which is a Typescript parameter property on the constructor. The service is provided to the component through the dependency injection system.
```typescript
export class HeroListComponent implements OnInit {
  heroes: Hero[];
  selectedHero: Hero;

  constructor(private service: HeroService) { }

  ngOnInit() {
    this.heroes = this.service.getHeroes();
  }

  selectHero(hero: Hero) { this.selectedHero = hero; }
}
```
Angular creates, updates, and destroys components as the user moves through the application. Your app can take action at each moment in this lifecycle through optional lifecycle hooks, like ngOnInit().

## Component Metadata
The `@Component` decorator identifies the class immediately below it as a component class, and specifies its metadata. In the example code below, you can see that *HeroListComponent* is just a class with no special Angular notation or syntax at all. It's not a component until you mark it as one with the `@Component` decorator.
The metadata for a component tells Angular where to get the major building blocks that it needs to create and present the component and its view. In particular, it associates a template with the component, either directly with inline code, or by reference. Together, the component and its template describe a view.
In addition to containing or pointing to the template, the @Component metadata configure, for example, how the component can be referenced in HTML and what services it requires.
```typescript
@Component({
  selector:    'app-hero-list',
  templateUrl: './hero-list.component.html',
  providers:  [ HeroService ]
})
export class HeroListComponent implements OnInit {
/* . . . */
}
```
This example shows some of the most useful `@Component` configuration options
- *selector*: A CSS selector that tells Angulae to create and insert an instances of this component wherever it finds the corresponding tag in template HTML. For example, if an app's HTML contains <app-hero-list></app-hero-list>, then Angular inserts an instance of the HeroListComponent view between those tags.
- *templateUrl*: The module-relative address of this component's HTML template. Alternatively, you can provide the HTML template inline, as the value of the template property. This template defines the component's *host view*.
- *providers*: An array of providers for services that the component requires. In the example, this tells Angular how to provide the HeroService instance that the component's constructor uses to get the list of heroes to display. 

## Templates and views
You define a component's view with its companion template. A template is a form of HTML that tells Angular how to render the component.
Views are typically arranged hierarchically, allowing you to  modify or show and hide entire UI sections or pages as a unit. The template immediately associated with a component defines that component's *host view*. The component can also define a *view hierarchy*, which contains *embedded views*, hosted by other components.

A view hierarchy can include views from component in the same NgModule, but it also can (and often does) include views from components that are defined in different NgModules.

## Template syntax
A template looks like regular HTML, except that it also contains Angular template syntax, which alters the HTML based on your app's logic and the state of app and DOM data, pipes to transform data before it is displayed.
For example, here is a template for the HeroListComponent.
```typescript
<h2>Hero List</h2>

<p><i>Pick a hero from the list</i></p>
<ul>
  <li *ngFor="let hero of heroes" (click)="selectHero(hero)">
    {{hero.name}}
  </li>
</ul>

<app-hero-detail *ngIf="selectedHero" [hero]="selectedHero"></app-hero-detail>
``` 

