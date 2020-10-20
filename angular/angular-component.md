# Component
Component are the most basic UI building block of an Angular app.

# Pipes
Use pipes to transform strings, currency amounts, dates and other data for display. Pipes are simple function you can use in template expressions to accept an input and return a  transformed value. Pipes are useful because you can use them throughout your application, while only declaring each pipe once. For example, you would use a pipe to show a data as `April 15, 1988` rather than the raw string format.

Angular provides built-in pipes for typical data transformation, including transformations for internationalization (i18n), which use locale information to format data. The following are commonly used built-in pipes for data formatting.
- DatePipe: Formats a data value according to locale rules.
- UpperCasePipe: Transforms text to all upper case.
- LowerCasePipe: Transforms text to all lower case.
- CurrencyPie: Transform a number to a currency string, formatted according to locale rules.
- DecimalPipe: Transform a number into string with a decimal point, formatted according to locale rules.
- PercentPipe: Transforms a number to a percentage string, formatted according to locale rules.

## Using a pipe in a template
To apply a pipe, use the pipe operator(|) within a template expression as shown in the following code example, along with the name of the pipe, which is date for the built-in DatePipe. 

```javascript
<p>The hero's birthday is {{ birthday | date }}</p>
```

## Transforming data with parameters and chained pipes
Use optional parameters to fine-tune a pipe's output. For examplem you can use the CurrencyPipe with a country code such as EUR as a parameter. The template expression `{{amount | currency: 'EUR'}}` transform the amount to currency in euros. Follow the pipe name(currency) with a code (:) and the parameter value ('EUR').

If the pipe accepts multiple parameters separate the values with colons. For example, `{{amount | |curreny : 'EUR':'Euros'}}` adds the second parameter, the string literal 'Euros' to the output string string. You can use any valid template expression as a parameter, such as a string literal or a component property.

Some pipes requires at least one parameter and allow more optional parameters usch as SlicePipe. For example, {{slice:1:5}} creates a new array or string containing a subset of the element starting with element 1 and ending with element 5.

You can chain pipes so that the output of one pipe becomes the input to the next.
In the following example, chained pipes first apply a format to a date value, then convert the formatted date to uppercase characters. 
```javascript
The chained hero's birthday is
{{ birthday | date | uppercase}}
```
## Creating pipes for custom data transformation
Create custom pipes to encapsulate transformation that are not provided with the built-in-pipes. You can use your custom pipe in template expression, the same way you use built-in-pipes-to transform input values to output values for display.

### Marking a class as a pipe
To mark a class as a pipe and supply configuration metadata apply the `@Pipe` decorator to the class. Use `UpperCamelCase` for the pipe class name and camelCase for the corresponding name string. Do not use hyphens in the name. 

### Using the PipeTransform interface
Implement the `PipeTransform` interface in your custom pipe class to perform the transformation. 

Angular invokes the transform method with the value of a binding as the first argument, and any parameters as the second argument in list form, and returns the transformed value.

Example - Transforming a value exponetially
```javascript
import { Pipe, PipeTransform } from '@angular/core';
/*
 * Raise the value exponentially
 * Takes an exponent argument that defaults to 1.
 * Usage:
 *   value | exponentialStrength:exponent
 * Example:
 *   {{ 2 | exponentialStrength:10 }}
 *   formats to: 1024
*/
@Pipe({name: 'exponentialStrength'})
export class ExponentialStrengthPipe implements PipeTransform {
  transform(value: number, exponent?: number): number {
    return Math.pow(value, isNaN(exponent) ? 1 : exponent);
  }
}
```

### Detecting changes with data binding in pipes
You use data binding with a pipe to display values and respond to user actions. If the data is a primitive input value, such as String or Number, or an object reference as input, such as Date or Array. Angular executes the pipe whenever it detects a change for the input value or reference.

Angular detects each change and immediately runs the pipe. This is fine for primitive input values. However, if you change something inside a composite object(such as the month of a date, an element of an array, or an object property), you need to understand how change detection works, and how to use an `impure` pipe.

### How change detection works?
Angular looks for change to data-bound values in a change detection process that runs after every DOM event: every keystroke, mouse move, timer tick and server response. 

However, executing a pipe to update the display with every change would slow down your app's performance. So Angular uses a faster change-dectection algorithm for executing a pipe.

### Detecting pure changes to primitive and object reference.
By default, pipes are defined as pure so that Angular executes the pipe only when it detects a _pure change_ to the input value. A pure change is either a change to a primitive input value(Such as string, number, boolean or symbol) or changed object reference(such as Date, Array, Function or Object).

A pure pipe must use a pure function, which is one that processes inputs and return values without side effects. In other words, given the same input, a pure function should always return the same output.

With a pure pipe, Angular ignores changes within composite objects such as newly added element of an existing array, because checking a primitive value or object reference is much faster than performing a deep check for differences with objects. Angular can quickly determine if it can skip executing the pipe and updating the view.

However, a pure pipe with an array as input may not work the way you want. To demonstrate this issue, to filter the list of heroes to just those heroes who can fly. Use the FlyingHeroPipe in the *ngFor repeater.
```javascript
<div *ngFor="let hero of (heroes | flyingHeroes)">
  {{hero.name}}
</div>

import { Pipe, PipeTransform } from '@angular/core';

import { Flyer } from './heroes';

@Pipe({ name: 'flyingHeroes' })
export class FlyingHeroesPipe implements PipeTransform {
  transform(allHeroes: Flyer[]) {
    return allHeroes.filter(hero => hero.canFly);
  }
}
```

The app now shows unexpected behavior: When the user adds flying heroes, none of them appear under "Heroes who fly". This happend because the code that adds a hero does so by pushing it onto the heroes array.

The change dector ignores changes to element of an array, so the pipe doesn't run.

The reason Angular ignores the changed array element is that the reference to array hasn't changed. Sine the array is the same. Angular does not update the display.

One way to get the behavior you want is to change the object reference itself. You can replace the array with a new array containing the newly changed elements, and then input the new array to the pipe. In the aboe example, you can create an array with the new hero appended, and assign that to heroes. Angular detects the change in the array reference and executes the pipe.

To summarize, if you mutate the input array, the pure pipe doesn't execute. If you replace the input array the pipe executes and the display is updated.

To keep your component simpler and independent of HTML templates that uses pipes, you can as an alternative, use an impure pipe to detect changes within composite objects such as arrays.

### Detecting impure changes within composite objects.
To execute a custom pipe after a change within a composite object, such as a change to an element of an array, you need to define your pipes as _impure_ to detect impure changes. Angular executes an impure pipe every time it detect a change with every keystroke and mouse movement.

Make a pipe impure by setting its _pure_ flag to _false_:
```javascript
@Pipe({
  name: 'flyingHeroesImpure',
  pure: false
})
```

## Unwrapping data from a observable
Observable let you pass messages between parts of your application. Observables are recommended for event handling, asynchronous programming and handling multiple values. Observables can deliver single or multiple values of any type, either synchronously(as a function delivers a value to its caller) or asynchronously on a schedule.

Use the built-in AsyncPipe to accept an observable as input and subscribe to the input automatically. Without this pipe, your component code would have to subscribe to the observable to consume its values, extract the resolved values, expose them for binding and unsubscribe when the observable is destroyed in order to prevent memory leaks. AsyncPipe is an impure pipe that save boilerplate code in your component to maintain the subscribtion and keep delivering values from  that observable as they arrive.

```javascript
import { Component } from '@angular/core';

import { Observable, interval } from 'rxjs';
import { map, take } from 'rxjs/operators';

@Component({
  selector: 'app-hero-message',
  template: `
    <h2>Async Hero Message and AsyncPipe</h2>
    <p>Message: {{ message$ | async }}</p>
    <button (click)="resend()">Resend</button>`,
})
export class HeroAsyncMessageComponent {
  message$: Observable<string>;

  private messages = [
    'You are my hero!',
    'You are the best hero!',
    'Will you be my hero?'
  ];

  constructor() { this.resend(); }

  resend() {
    this.message$ = interval(500).pipe(
      map(i => this.messages[i]),
      take(this.messages.length)
    );
  }
}
```

### Caching HTTP requests
To communicate with backend services using HTTP, the HttpClient service uses observables and offers the HttpClient.get() method to fetch data from a server. The asynchronous method sends a HTTP request and returns an observable that emits the requested data for the response.

As shown in the previous section, you can use the impure AsyncPipe to accept an observable as input automatically. You can also create an impure pipe to make and cache an HTTP request.

Impure pipes are called whenever changes detection runs for a computer, which could be every few milliseconds for CheckAlways. To avoid performance problems, call the server only when the requested URL changes as shown in the following example, and use the pipe to cache the server response.

```javascript
//fetch-json.pipe.ts
import { HttpClient } from '@angular/common/http';
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'fetch',
  pure: false
})
export class FetchJsonPipe implements PipeTransform {
  private cachedData: any = null;
  private cachedUrl = '';

  constructor(private http: HttpClient) { }

  transform(url: string): any {
    if (url !== this.cachedUrl) {
      this.cachedData = null;
      this.cachedUrl = url;
      this.http.get(url).subscribe(result => this.cachedData = result);
    }

    return this.cachedData;
  }
}
//hero-list.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-hero-list',
  template: `
    <h2>Heroes from JSON File</h2>

    <div *ngFor="let hero of ('assets/heroes.json' | fetch) ">
      {{hero.name}}
    </div>

    <p>Heroes as JSON:
      {{'assets/heroes.json' | fetch | json}}
    </p>`
})
export class HeroListComponent { }
```
In the above example, a breakpoint on the pipe's request for data shows the following:
- Each binding gets its own pipe instance
- Each pipe instance caches its own URL and data and calls the server only once.

# Component Lifecycle
A component instance has a lifecycle that starts when Angular instantiates the component class and renders the component view along with its child views. The lifecycle continues with change detection, as Angular checks to see when data-bound properties change, and updates both the view and the component instance as needed. The lifecycle ends when Angular destroys the component instance and removes its rendered template from the DOM. Directives have a similar lifecycle, as Angular creates, updates and destroy instances in the course of execution.

Your application can use lifecycle hooks methods to tap into key events in the lifecycle of a component or directive in order to initialize new instances initiate change detection when needed, respond to updates during change detection and clean up before deletion of instances.

## Responding to lifecycle events
You can respond to events in the lifecycle of a component or directive by implementing one or more of the lifecycle hook interfaces in the Angular core library. The hook give you the opporunity to act on a component or directive instance at the appropriate moment, as Angular creates , updates or destroys that instance.

Each interfaces defines the prototype for a single hook method, whose name is the interface name prefixed with ng. For examplem the OnInit interface has a hook method named ngOnInit(). If you implement this method in your component or directive class. Angular class, it shortly after checking the input properties for that component or directive for the first time.

## Lifecycle event sequence
After your application instantiates a component or directive by calling its constructor, Angulr calls the hook methods you have implemented at the appropriate poit in the lifecycle of that instance.

Angular executes hook methods in the following sequence. You can use them to perform the following kinds of operations.

|**Hook Method**| **Purpose**| **Timing**|
|---------------|------------|-----------|
|_ngOnChanges()_| Respond when Angular set or reset data-bound input properties. The method receives a SimpleChange object of current and previous property values. | Called before _ngOnInit()_ and whenever one or more data-bound inpur properties change.|
|_ngOnInit()_| Initialize the directive or component after Angular first displays the data-bound properties and set the directive or component's input properties.| Called once, after the first ngChanges().|
|_ngDoCheck()_| Detect and act upon that Angular can't or won't detect on its own.| Called immediately after _ngChanges()_ on every change detection run and immediately after _ngOnIniti()_ on the first run.|
|_ngAfterContentInit()_| Respond after Angular project external content into the components view, or into the view that a directive is in.| Called once after the first _ngDoCheck()_.|
|_ngAfterContenctChecked()_| Respond after Angular check the content project into the directive or component.|
|_ngAfterViewInit()_| Respond after Angular intialize the components view and child views. or the view that contains the directive.|
|_ngAfterViewChecked()_| Respond after Angular checks the components views and child views or the view that contains the directive.|

# View Encapsulation
In Angular, component CSS styles are encapsulated into the component's view and don't affect the rest of the application. 

To control how this encapsulation happens on a per component basis , you can set the view encapsulation mode in the component metadata. Choose from following modes:
- ShadowDom View encapsulation uses the browser's native shadow DOM implementation to attach a shadow DOM to the component's host element and then puts the component view inside that shadow DOM. The component's style are included within the shadow DOM.
- Native view encapsulation uses a now deprecated version of the browser's native shadow DOM implementation.
- Emulated view encapsulation (the default) emulates the behavior of shadow DOM by preprocessing (and remaining) the CSS code to effectively scope the CSS to the component's view.
- None means that Angular does no view encapsulation. Angular adds the CSS to the global styles. The scoping rules, isolations and protections discussed earilier don't apply. This is essentially the same as pasting the component's style into the HTML.
## Inspecting generated CSS
When using emulated view encapsulation, Angular preporcesses all component styles so that they approximate the standard shadow CSS scoping rules.
In the DOM of a running Angular application with emulated view encapsulation enabled, each DOM element has some extra attributes attached to it:
```html
<hero-details _nghost-pmm-5>
  <h2 _ngcontent-pmm-5>Mister Fantastic</h2>
  <hero-team _ngcontent-pmm-5 _nghost-pmm-6>
    <h3 _ngcontent-pmm-6>Team</h3>
  </hero-team>
</hero-detail>
```
There are two kinds of generated attributes:
1. An element that would be a shadow DOM host in native encapsulation has a generated _nghost attribute. This is typically the case for component host elements.
2. An element within a component's view has a _ngcontent attribute that identifies to which host's emulated shadow DOM this element belongs.

The exact values of these attributes aren't important. They are automatically generated and you never refer to them in application code. But they are targeted by the generated component styles, which are in the &lt;head&gt; section of the DOM.
```css
[_nghost-pmm-5] {
  display: block;
  border: 1px solid black;
}

h3[_ngcontent-pmm-6] {
  background-color: white;
  border: 1px solid #777;
}
```
These styles are post-processed so that each selector is augmented with _nghost or _ngcontent attribute selectors. These extra selectors enable the scoping rules described in this page.

# Component Interaction
1. Pass data from parent to child with input binding(@Input decorator).
2. Intercept input property changes with a setter. Use an input property setter to intercept and act upon a value from the parent.
3. Intercept input property changes with ngOnChanges()
4. Parent listens for child event
5. Parent interact with child via local variable. Yo can do both by creating a template reference variable for the child element and then reference that variable within the parent template.
6. Parent calls an @ViewChild(). The local variable approach is simple and easy. But it is limited because the parent-child wiring be done entirely withing the parent template. The parent component itself has no access to the child.
7. Parent and children communicate via a service. A parent component and its children share a service whose interface enables bi-directional communication within the family.

# Component Styles
Angular applications are styled with standard CSS. That means you can apply everthing you know about CSS stylesheets, selectors, rules and media queries directly to Angular applications.

Additionally, Angular can bundle componenet styles with components, enabling a more modular design that regular stylesheets.

## Using component styles
For every Angular component you write, you may define not only an HTML template, but also the CSS styles that go with that template, specifying any selectors, rules and media queries that you need.

One way to do this is to set the _style_ property in the component metadata. The _styles_ property takes an array of strings that contains CSS code. Usually you give it one string, as in the following example:
```javascript
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styles: ['h1 { font-weight: normal; }']
})
export class HeroAppComponent {
/* . . . */
}
```
## Style Scope
They are not inherited by an components nested within the template nor by any content projected into the component.

In this example, that `h1` style applies only to the HeroAppComponent, not to the nested HeroMainComponent nor to &lt;h1&gt; tags anywhere else in the application.

This scoping restriction is a styling modularity feature:
- You can use the CSS class names and selectors that make the most sense in the context of each component.
- Class names and selectors are local to the component and don't collide which classes and selectors used elsewhere in the application.
- Changes to styles elsewhere in the application don't affect the component's styles.
- You can co-locate the CSS code of each component with the Typescript and HTML code of the component, which leads to a neat and tidy project strcuture.
- You can change or remove component CSS code without searching through the whole application to find where else the code is used.

## Special Selectors
Component styles have a few special selectors from the world of shadow DOM style scoping. The following sections describe these selectors.
### :host
Use the :host pseudo-class selector to target styles in the element that hosts the component(as opposed to targeting element inside the component's template).
```css
:host {
  display: block;
  border: 1px solid black;
} 
```
The :host selector is the only way to target the host element. You can't reach the host element from inside the component with other selectors because it's not part of the component's own template. The host element is in a parent component's template.

Use the function form to apply host styles conditionally by including another selector inside parentheses after :host.

The next example targets the host element again, but only when it also has the active CSS class.
```css
:host(.active) {
  border-width: 3px;
}
```
### :host-context
Sometimes it's useful to apply style based on some condition outside of a component's view. For example, a CSS theme class could be applied to the document&lt;body&gt; element, and you want to change how your component looks based on that.

Use the `:host-content()` pseudo-class selector, which works just like the function form of :host(). The :host-context() selector looks for a CSS class in any ancestor of the component host element, up to the document root. The :host-contect() selector is useful when combined with another selector.

The following example applies a background-color style to all &lt;h2&gt; elements inside the component, only if some ancestor element has the CSS class _theme-light_.

```css
:host-context(.theme-light) h2 {
  background-color: #eef;
}
```
### (deprecated) /deep/, >>>, and ::ng-deep
Component styles normally apply only to the HTML in the component's own template.

Applying the ::ng-deep pseudo-class to any CSS rule completed disables view-encapsulation for that rule. Any style with ::ng-deep around become a global style. In order to scope the specified style to the current component and all its descendants, be sure to include the :host selector before ::ng-deep. If the ::ng-deep combinator is used without the :host pseudo-class selector, the style can bleed into other components.

The following example target all &lt;h3&gt; elemenets, from the host element down through this component to all of its child element in the DOM.
```css
:host /deep/ h3 {
  font-style: italic;
}
```
The /deep/ combinator also has the aliases >>>, and  ::ng-deep.

> Use /deep/, >>>, and ::ng-deep only with emulated view encapsulation. Emulated is the default and most commonly used view encapsulation.

> The shadow-piercing descendant combinator is deprecated and support is being remoed from major browsers and tools. As such we plan to drop support in Angular (for all 3 of /deep/, >>>, and ::ng-deep). Until then ::ng-deep should be prefered for a border compatiability with the tools.

## Loding component styles
There are several ways to add styles to a component:
- By setting styles or styleUrls metadata
- Inline in the template HTML.
- With CSS imports.

The scoping rules outlines earilier apply to each of these loading patterns.

### Styles in component metadata
You can add a styles array property to the @Component decorator.

Each String in the array defines some CSS for this component.

```javascript
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styles: ['h1 { font-weight: normal; }']
})
export class HeroAppComponent {
/* . . . */
}
```
The Angular CLI command `ng generate component` defines an empty styles array when you create the component with the --inline-style flag.

> ng generate component hero-ap --inline-style

### Style files in component metadata
You can load styles from external CSS files by adding a styleUrls property to a component's @Component decorator:

```css
h1 {
  font-weight: normal;
}
```
```javascript
@Component({
  selector: 'app-root',
  template: `
    <h1>Tour of Heroes</h1>
    <app-hero-main [hero]="hero"></app-hero-main>
  `,
  styleUrls: ['./hero-app.component.css']
})
export class HeroAppComponent {
/* . . . */
}
```

> You can specify more than one styles files or even combination of styles and styleUrls.

When you use the Angular CLI command ng generate componet without the --inline-style flag, it creates an empty styles file for you and references that file in the component generated styleUrls.

> ng generate component hero-app

### Template inline styles
You can embed CSS styles directly into the HTML template by putting them inside &lt;style&gt; tag.

```javascript
@Component({
  selector: 'app-hero-controls',
  template: `
    <style>
      button {
        background-color: white;
        border: 1px solid #777;
      }
    </style>
    <h3>Controls</h3>
    <button (click)="activate()">Activate</button>
  `
})
```
### Tempate link tags
You can also write &lt;link&gt; tags into the component's HTML element.

```javascript
@Component({
  selector: 'app-hero-team',
  template: `
    <!-- We must use a relative URL so that the AOT compiler can find the stylesheet -->
    <link rel="stylesheet" href="../assets/hero-team.component.css">
    <h3>Team</h3>
    <ul>
      <li *ngFor="let member of hero.team">
        {{member}}
      </li>
    </ul>`
})
```

### CSS @imports
You can use also import CSS files into the CSS files using the standard CSS @import rule. 

In this case, the URL is relative to the CSS file into which your are importing

```css
/* The AOT compiler needs the `./` to show that this is local */
@import './hero-details-box.css';
```

### External and global style files
When building with the CLI, you must configure the angular.json to include all external assets, including external style files.

Register global style files in the styles section which by default, is pre-configured with the global style.css file.

### Non-CSS style files
If you're building with the CLI, you can write style files in sass, less or stylus and specify those files in the @Component.styleUrls metadata with appropriate extensions (.scss, .less, styl) as in the following example.
```javascript
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
...
```
The CLI build process runs the pertinent CSS preprocessor. 

When  generating a component file with ng generate component, the CLI emits an empty CSS styles file (.css) by default. You can configure the CLI to default to you preferred CSS preprocessor.

> Styles string added to the @Component.styles are must be written in CSS because the CLI cannot apply a preprocessor to inline styles.

# @Input & @Output Properties
@Input() and @Output() allow Angular to share data between the parent context and child directives or components. An @Input() property is writable while an @Output() property is observable.

Consider this example of a child/parent relationship:
```html
<parent-component>
  <child-component></child-component>
</parent-component>
```

Here, the &lt;child-component&gt; selector or child directive, is embedded within a &lt;parent-component&gt;, which servers as the child's context.

@Input() and @Output() act as the API, or application programming interface, of the child component in that they allow the child to communicate with the parent. Think of @Input() and @Output() like ports or doorways - @Input() is the doorway into the component allowing to data to flow in while @Output() is the doorway out of the component, allowing the child component to send data out.

## How to use @Input()
Use the @Input() decorator in a child component or directive to let Angular know that a property in that component can receive its value from it parent component. It helps to remember that the data flow is from the prespective of the child component. So an @Input() allows data to be input into the child component from the parent component.

![How to use @Input](https://angular.io/generated/images/guide/inputs-outputs/input.svg)

To illustrate the use of @Input(), edit these parts of your app:
- The child component class and template.
- The parent component class and template.

### In the Child
To use the @Input() decorator in a child component class, first import Input and then decorate the property with @Input():

```javascript
import { Component, Input } from '@angular/core'; // First, import Input
export class ItemDetailComponent {
  @Input() item: string; // decorate the property with @Input()
}
```

In this case, @Input() decorates the property item which has a type of string, however @Input() properties can have any type, such as number, string, boolean or object. The value for item will come from the parent component, which the next section covers.

Next, in the child component template, add the following:
```javascript
<p>
  Today's item: {{item}}
</p>
```

### In the parent
The next step is to bind the property in the parent component's template. In this example, the parent component template is `app.component.html`.

First, use the child's selector here `<app-item-detail>`, as a directive within the parent component template. Then use property binding to bind the property in the child to the property of the parent.

```html
<app-item-detail [item]="currentItem"></app-item-detail>
```
Next, in the parent component class, `app.component.ts` designate a value for currentItem.

```javascript
export class AppComponent {
  currentItem = 'Television';
}
```
With @Input(), Angular passes the value for currentItem to the child so that item renders as Television.

## How to use @Output()
Use the @Output() decorator in the child component or directive to allow data to flow from the child out to the parent.

An @Output() property should normally be initialized to an Angular EventEmitter with values flowing out of the component as events.

![How to use @Output](https://angular.io/generated/images/guide/inputs-outputs/output.svg)

Just like with @Input(), you can use @Output() on a property of the child component but its type should be EventEmitter.

@Output() marks as a property in a child component as a doorway through which data can travel from the child to the parent. The child component then has to raise an event so the parent knowns something has changed. To raise an event @Output() works hand in hand with EventEmitter, which is a class in `@angular/core` that you use to emit custom events.

When you use @Output() edit these parts of your app:
- The child component class and template
- The parent component class and template

The following example shows how to set up an @Output() in a child component that pushes data you enter in an HTML &lt;input&gt; to an array in the parent component.

### In the child
This example features an &lt;input&gt; where a user can enter a value and click a &lt;button&gt; that raises an event. The EventEmitter then relays the data to the parent component.

First, be sure to import `Output` and `EventEmitter` in the child component class:
```javascript
import { Output, EventEmitter } from '@angular/core';
```
Next, still in child decorate a property with @Output() in the component class. The following example @Output() is called newItemEvent and its type is EventEmitter, which means it's an event.

```javascript
@Output() newItemEvent = new EventEmitter<string>();
```
The different parts of the above declaration are as follows:
- @Output() - a decorator function marking the property as a way for data to go from the child to the parent.
- newItemEvent - the name of the @Output.
- EventEmitter<string> - the @Output type.
- new EventEmitter<string>() - tells Angular to create a new event emitter and that the data it emits is of type string. The type could be any type such as number, boolean and so on.

Next, create an addNewItem() method in the same component class:

```javascript
export class ItemOutputComponent {

  @Output() newItemEvent = new EventEmitter<string>();

  addNewItem(value: string) {
    this.newItemEvent.emit(value);
  }
}
```
The addNewItem() function uses the @Output(), newItemEvent, to raise an event in which it emits the value the user type into the &lt;input&gt;. In other words, when the user clicks the add button in the UI, the child lets the parent know about the event and gives that data to the parent. 

### In the child's template
The child's template has two controls. The first is an HTML &lt;input&gt; with a template reference variable, #newItem, where the user type is an item name. Whatever the user types into the &lt;input&gt; get stored in the value property of the #newItem variable.

```html
<label>Add an item: <input #newItem></label>
<button (click)="addNewItem(newItem.value)">Add to parent's list</button>
```
The second element is a &lt;button&gt; with an event binding. You know it's an event binding because the part of the left of the equal sign is in parenthese (click).

The (click) event is bound to the addNewItem() method in the child component class which takes as its argument whatever the value of #newItem.value property is.

Now the child component has an @Output() for sending data to the parent and a method for raising an event. The next step is in the parent.

### In the parent
In this example, the parent component is AppComponent, but you could use any component in which you could nest the child.

The AppComponent in this example features a list of items in an array and a method for adding more items to the array.

```javascript
export class AppComponent {
  items = ['item1', 'item2', 'item3', 'item4'];

  addItem(newItem: string) {
    this.items.push(newItem);
  }
}
```
The addItem() method takes an argument in the form of a string and then pushes or adds that string to the items array.

### In the parent's template
Next, in the parent template, bind the parent's method to the child event. Put the child selector, here &lt;app-item-output&gt;, within the parent component's template, `app.component.html`.

```html
<app-item-output (newItemEvent)="addItem($event)"></app-item-output>
```
The event binding `(newItemEvent) = 'addItem($event)'`, tells Angular to connect the event in the child, newItemEvent, to the method in the parent, addItem(), and that the event that the child is notifying the parent about is to be the argument of addItem(). In other words this is where the actual hand off of data takes place. The $event contains the data that the user types into the &lt;input&gt; in the child template UI.
Now, in order to see the @Output() working add the following to the parent template:

```html
<ul>
  <li *ngFor="let item of items">{{item}}</li>
</ul>
```

## @Input() and @Output together
You can use @Input() and @Output() on the same child component as in the following:

```html
<app-input-output [item]="currentItem" (deleteRequest)="crossOffItem($event)"></app-input-output>
```
The target item, which is an @Input() property in the child component class, receives its value from the parent's property, currentItem. When you click delete, the child component raises an event, deleteRequest, which is the argument for the parent's `crossOffItem()` method. 

The following diagram is of an @Input() and an @Output() on the same child component and shows the different parts of each:

![input-output-diagram](https://angular.io/generated/images/guide/inputs-outputs/input-output-diagram.svg)

As the diagram shows, use inputs together in the same manner as using them separately. Here, the child selector is &lt;app-input-output&gt; with item and deleteRequest being @Input() and @Output properties in the child component class. The property currentItem and the method crossOfItem() are both in the parent component class.

To combine property and event binding using the banana-in-a-box syntax, [()]

## @Input() and @Output() declarations
Instead of using the @Input() and @Output() decorators to declare inputs and outputs, you can identify members in the inputs and outputs arrays of the directive metadata, as in this example:

```javascript
// tslint:disable: no-inputs-metadata-property no-outputs-metadata-property
inputs: ['clearanceItem'],
outputs: ['buyEvent']
// tslint:enable: no-inputs-metadata-property no-outputs-metadata-property
```
While declaring inputs and outputs in the @Directive and @Component metadata is possible, it is a better pratice to use the @Input() and @Output() class decorators instead, as follows:

```javascript
@Input() item: string;
@Output() deleteRequest = new EventEmitter<string>();
```
> If you get a template parse error when trying to use inputs or outputs, but you know that the properties do indeed exists, double check that your properties are annotated with @Input()/@Output() or that you've declared them in an inputs/outputs array.
>
> Uncaught Error: Template parse errors:
>
> Can't bind to 'item' since it isn't a known property of 'app-item-detail'

## Aliasing inputs and outputs
Sometimes the public name of an input/output should be different from the internal name. White it is a best pratice to avoid this situation. Angular does offer a solution.
### Aliasing in the metadata
Aliasing inputs and outputs in the metadata using a colon-delimited(:) string with the directive property name on the left and public alias on the right.

```javascript
// tslint:disable: no-inputs-metadata-property no-outputs-metadata-property
inputs: ['input1: saveForLaterItem'], // propertyName:alias
outputs: ['outputEvent1: saveForLaterEvent']
// tslint:disable: no-inputs-metadata-property no-outputs-metadata-property
```
### Aliasing with the @Input()/@Output() decorator
You can specify the alias for the property name by passing the alias name to the @Input()/@Output() decorator. The internal name remains as usual.
```javascript
@Input('wishListItem') input2: string; //  @Input(alias)
@Output('wishEvent') outputEvent2 = new EventEmitter<string>(); //  @Output(alias) propertyName = ...
```

# Dynamic component loader
Component templates are not always fixed. An application may need to load new components at runtime.

The following example shows how to build a dynamic ad banner.

The hero agency is planning an ad campaign with several different ads cycling through the banner. New ad components are added frequently by serveral different teams. This makes it impratical to use a template with a static component structure.

Instead, you need a way to load a new component without a fixed reference to the component in the ad banner's template.

Angular comes with its own API for loading components dynamically.

### The anchor directive
Before you can add components you have to define an anchor point to tell Angular where to insert components.

The ad banner uses a helper directive called `AdDirective` to mark valid insertion points in the template.

```javascript
import { Directive, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[ad-host]',
})
export class AdDirective {
  constructor(public viewContainerRef: ViewContainerRef) { }
}
```
AdDirective injects ViewContainerRef to gain access to the view container of the element that will host the dynamically added component.

In the @Directive decorator, notice the selector name, ad-host; that's what you use to apply the directive to the element.

### Loading Components
Most of the ad banner implementation is in ad-banner.component.ts. To keep thing simple in this example, HTML is in the @Component decorator's template property as a template string.

The `<ng-template>` element is where you apply the directive you just made. To apply the AdDirective, recall the selector from ad.directive.ts, ad-host. Apply that to `<ng-template>` without the square brackets. Now Angular knows where to dynamically load components.

```javascript
template: `
            <div class="ad-banner-example">
              <h3>Advertisements</h3>
              <ng-template ad-host></ng-template>
            </div>
          `
```

The `<ng-template>` element is a good choice for dynamic components because it doesn't render any additional output.

### Resolving components
Take a closer look at the methods in ad-banner.component.ts

AdBannerComponent takes an array of AdItem objects as input, which ultimately comes from AdService AdItem objects specify the type of component to load and any data to bind to the component. AdService returns the actual ads making up the ad campaign.

Passing an array of components to AdBannerComponent allows for a dynamic list of ads without static elements in the template.

With its getAds() method, AdBannerComponent cycles through the array of AdItems and loads a new component every 3 seconds by calling loadComponent().

```javascript
export class AdBannerComponent implements OnInit, OnDestroy {
  @Input() ads: AdItem[];
  currentAdIndex = -1;
  @ViewChild(AdDirective, {static: true}) adHost: AdDirective;
  interval: any;

  constructor(private componentFactoryResolver: ComponentFactoryResolver) { }

  ngOnInit() {
    this.loadComponent();
    this.getAds();
  }

  ngOnDestroy() {
    clearInterval(this.interval);
  }

  loadComponent() {
    this.currentAdIndex = (this.currentAdIndex + 1) % this.ads.length;
    const adItem = this.ads[this.currentAdIndex];

    const componentFactory = this.componentFactoryResolver.resolveComponentFactory(adItem.component);

    const viewContainerRef = this.adHost.viewContainerRef;
    viewContainerRef.clear();

    const componentRef = viewContainerRef.createComponent(componentFactory);
    (<AdComponent>componentRef.instance).data = adItem.data;
  }

  getAds() {
    this.interval = setInterval(() => {
      this.loadComponent();
    }, 3000);
  }
}
```

The loadComponent() method is doing a lot of the heavy lifting here. Take it step by step. First ,it picks an ad.

After loadComponent() selects an ad, it uses ComponentFactoryResolver to resolve a ComponentFacotry for each specific component. The ComponentFactory then creates an instance of each component.

Next you're targeting the viewContainerRef that exists on this specific instance of the component. How do you know it's this specific instance? Because it's referring to adHost and adHost is the directive you set up earilier to tell Angular where to insert dynamic components.

As you may recall, AdDirective injects ViewContainerRef into its constructor. This is how directive accesses the element that you want to use to host the dynamic component.

To add the component to the template, you call createComponent() on ViewContainerRef.
The createComponent() method returns a reference to the loaded component. Use that reference to interact with component by assigning to its properties or calling its methods.

### Selector references
Generally, the Angular compiles generate a ComponentFactory for any component referenced in a template. However, there are no selector references in the templates for dynamically loaded components since they loaded at runtime.

To ensure that the compiler still generates a factory, add dynamically loaded components to the NgModule's entryComponents array:
```javascript
entryComponents: [ HeroJobAdComponent, HeroProfileComponent ],
```

### The AdComponent interface
In the ad banner, all components implement a common AdComponent interface to standardize the API for passing data to the components.  

Here are two sample components and the AdComponent interface for reference.

```javascript
// hereo-job-ad.component.ts
import { Component, Input } from '@angular/core';

import { AdComponent }      from './ad.component';

@Component({
  template: `
    <div class="job-ad">
      <h4>{{data.headline}}</h4>

      {{data.body}}
    </div>
  `
})
export class HeroJobAdComponent implements AdComponent {
  @Input() data: any;

}

// hero-profile.component.ts
import { Component, Input }  from '@angular/core';

import { AdComponent }       from './ad.component';

@Component({
  template: `
    <div class="hero-profile">
      <h3>Featured Hero Profile</h3>
      <h4>{{data.name}}</h4>

      <p>{{data.bio}}</p>

      <strong>Hire this hero today!</strong>
    </div>
  `
})
export class HeroProfileComponent implements AdComponent {
  @Input() data: any;
}

// ad.component.ts
export interface AdComponent {
  data: any;
}
```
# Angular element
Angular elements are Angular components packaged as custom elements (also called Web components), a web standard for defining new HTML elements in a framework-agnostic way.

Custom elements are a Web platform feature currently supported by Chrome, Edge(Chromium-based), Firefox, Opera, and Safari, and available in other browsers through polyfills(see Browser Support). A custom elements extends HTML by allowing you to define a tag whose content is created and controlled by javscript code. The browser maintains a CustomElementRegistry of defined custom elements, which maps an instantiable Javascript class to an HTML tag.

The `@angular/element` package exports a createCustomElement() API that provides a bridge from Angular's component interface change detection functionality to the built-in DOM API.

Transforming a component to a custom element makes all of the required Angular infrastructure available to the browser. Creating a custom element is simple and straightforward, and automatically connects your component-defined view with change detection and data binding mapping Angular functionality to the corresponding native HTML equivalents.

> We are working on custom elements that can be used by web apps built on other frameworks. A minimal, self-contained version of the Angular framework will be injected as a service to support the component's change-detection and data-binding functionality.

## Using custom elements
Custom elements bootstrap themselves - they start automatically when they are added to the DOM, and are automatically destroyed when removed from the DOM. Once a custom element is added to the DOM for any page, it looks and behaves like any other HTML element, and does not require any special knowledge of Angular terms or usage conventions.
- **Easy dynamic content in an Angular app**
  Transforming a component to a custom element provides an easy path to creating dyanmic HTML content in your Angular app. HTML content that you add directly to the DOM in an angular app is normally displayed without Angular processing unless you define a dynamic component, adding your own code to connect the HTML tag to your app data, and participate in change detection. With a custom element. all of that wiring is taken care of automatically.
- **Content-rich application**  
  If you have a content-rich app, such as the Angular app that present this documentation, custom element let you give your content providers sophisticated Angular functionality without requiring knowledge of Angular. For example, an Angular guide like this one is added directly to the DOM by the Angular navigation tools, but can include special elements like `<code-snippet>` that perform complex operations. All you need to tell your content provider is the syntax of your custom element. They don't need to know anything about Angular, or anything about your component's data structure or implementation.

  ## How it works?
  Use the `createCustomElement()` function to convert a component into a class that can be registered with the browser as a custom element. After you register your configured class with the browser's custom-element registry, you can use the new element just like a built-in HTML element in content that you add directly into the DOM:
  
  ```html
  <my-popup message="Use Angular!"></my-popup>
  ```
When  your custom element is placed on a page, the browser creates an instance of the registered class and it to the DOM. The content is provided by the component's template, which uses Angular template syntax, and is rendered using the component and DOM data. Input properties in the component correspond to input attributes for the element.

![Custom Element](https://angular.io/generated/images/guide/elements/customElement1.png)

## Transforming components to custom elements
Angular provides the `createCustomElement()` function for converting an Angular component, together with its dependencies, to a custom element. The function collects the component's observable properties, along with the Angular functionality the browser needs to create and destroy instances, and to detect and respond to changes.

The conversion process implements the `NgElementConstructor` interface, and creates a constructor class that is configured to produce a self-bootstrapping instance of your component.

Use a Javascript function customElements.define(), to register the configured constructor and its associated custom-element tag with the browser's CustomElementRegistry. When the browser encounters the tag for the registered element, it uses the constructor to create a custom-element instance.

![My custom element](https://angular.io/generated/images/guide/elements/createElement.png)

### Mapping
A custom element hosts an Angular component, providing a bridge between the data and logic defined in the component and standard DOM APIs. Component properties and logic maps directly to HTML attributes and browser's event system.
- The creation API parse the component looking for input properties, and defines corresponding attributes for the custom element. It transform the property names to make them compatitble with custom elements, which do not recognize case distinctions. The resulting attribute names use dash-separeate lowercase. For example, for a component with @Input("myInputProp") inputProp, the corresponding custom element defines an attribute an `my-input-prop`.
- Component outputs are dispatched as HTML Custom Events, with the name of the custom event matching the output name. For example, for a component with @Output() valueChanged = new EventEmitter(), the corresponding custom element will dispatch events with the named "valueChanged" and the emitted data will be stored on the event's detail property. If you provide an alias, that value is used; for example, @Output('myClick') clicks = new EventEmitter<string>(); results in dispatch events with name "myClick".

### Example: A Popup Service
Previously, when you wanted to add a component to an app at runtime, you had to define a dynamic component. The app module have to list your dynamic component under entryComponents, so that the app wouldn't expect it to be present at startup, and then you would have to load it attached it to an element in the DOM, and wire up all of the dependenices, change detection, and event handling, as described in Dynamic Component.

Using an Angular custom element makes the process much simpler and more transparent, by providing all of the infrastructure and framework automatically - all you have to do is define the kind of event handling you want.(You do still have to exclude the component from compilation, if you are not going to use it in your app.)

The Popup Service example app defines a component that you can either load dynamically or convert to a custom element.
- _popup.component.ts_ define a simple pop-up element that display an input message, with some animation and styling.
- _popup.service.ts_ creates an injectable service that provides two different way to invoke the PopupComponent as a dynamic component, or as a custom element. Notice how much more setup is required for the dynamic-loading method.
- _app.module.ts_ add the PopupComponent in the module's entryComponents list, to exclude it from compilation and avoid startups warning or errors.
- _app.component.ts_ defines the app's root component, which use the PopupService to add th pop-up to the DOM at run time. When the app runs, the root component's constructor converts PopupComponent to a custom element.

### Typings for custom elements
Generic DOM API, such as `document.createElement()` or `document.querySelector()`, return an element type that is appropriate for the specified arguments. For example, calling `document.createElement('a')` will return an `HTMLAnchorElement` which Typescript knowns has a `href` property. Simlarly `document.createElement('div')` will returns an `HTMLDivElement`, which Typescript knowns has no `href` property.

When called with unkown elements, such as a custom element name(pop-up element in our example), the methods will return a generic type, such as HTML Element, since Typescript can't infer the correct type of the return element.

Custom elements created with Angular extend NgElement(which in turn extends HTMLELement). Additionally, these custom elements will have a property for each input of the corresponding component. For example, our popup-element will have a message property of type string.

There are few options if you want to get correct types for your custom elements. Let's assume you create a `my-dialog` custom element based on the following component:

```javascript
@Component(...)
class MyDialog {
  @Input() content: string;
}
```
The most straight forward way to get accurate typings is to cast the return value of the relevant DOM methods to the correct type. For that, you can use the NgElement and WithProperties types (both exported from @angular/elemets):

```javascript
const aDialog = document.createElement('my-dialog') as NgElement & WithProperties<{content: string}>;
aDialog.content = 'Hello, world!';
aDialog.content = 123;  // <-- ERROR: TypeScript knows this should be a string.
aDialog.body = 'News';  // <-- ERROR: TypeScript knows there is no `body` property on `aDialog`.
```
This is a good way to quickly get Typescript features, such as type checking and autocomplete support, for your custom element. But it can get cumbersome if you need it in serveral places, because you have to cast the return type on every occurence.

An alternative way, that only requires defining each custom element's type once, is augmenting the HTMLElementTagNameMap which Typescript uses to infer the type of a returned element based on its tag name (for DOM methods such as document.createElement(), document.querySelector(), etc.):

```javascript
declare global {
  interface HTMLElementTagNameMap {
    'my-dialog': NgElement & WithProperties<{content: string}>;
    'my-other-element': NgElement & WithProperties<{foo: 'bar'}>;
    ...
  }
}
```

Now, Typescript can infer the correct type the same way it does for built-in elements:

```javascript
document.createElement('div')               //--> HTMLDivElement (built-in element)
document.querySelector('foo')               //--> Element        (unknown element)
document.createElement('my-dialog')         //--> NgElement & WithProperties<{content: string}> (custom element)
document.querySelector('my-other-element')  //--> NgElement & WithProperties<{foo: 'bar'}>      (custom element)
```