# Built-in directives
Angular offers two kinds of built-in directives `attribute directives` and `structural directives`.

## Built-in attributes directives
Attribure directive listen to and modify the behavior of other HTML element, attributes, properties and components. You usually apply them to elements as if there HTML attributes, hence the name.

Many NgModules such as RouterModule and the FormModule define their own attribute directives. The most common attribute directive are as follows:
- **NgClass** - adds and removes a set of CSS classes.
- **NgStyle** - adds and removes a set of HTML styles.
- **NgModel** - adds two-way data binding to an HTML form element.

## Built in structural directives
Structural directives are responsible for HTML layout. They shape or reshape the DOM's structure, typically by adding, removing and manipulating the host elements to which they are attached.

Common built-in structural directives:
- **NgIf** - conditionally creates or destroys subviews from the template.
- **NgFor** - repeat a node for each item in a list.
- **NgSwitch** - a set of directives that switch among alternative views.

# Attribute Directives
An attribute directive change the appearance or behavior of a DOM element.

There are three kinds of directive in Angular
1. Components - directives with a template
2. Structural directives - change the DOM layout by adding and removing DOM elements.
3. Attribute directives - change the appearance or behavior of an element, component or another directive.

## Build a simple attribute directive
An attribute directive minimally requires building a controller class annotate with `@Directive`, which specifies the selector that identifies attribute. The controller class implements the desired directive behavior.

Building a simple `appHighlight` attribute directive to set an element's background color when the user hovers over that element. You apply it like this:

```html
<p appHighlight>Highlight me!</p>
```

### Write the directive code
Create the directive class file in a terminal window with the CLI command `ng generate directive`.

The CLI creates src/app/highlight.directive.ts, a corresponding test file src/app/highlight.spec.ts and declares directive class in the root AppModule.

```javascript
import { Directive } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor() { }
}
```
The imported Directive symbol provides Angular the @Directive decorator.

The @Directive decorator's lone configuration property specifies the directives CSS attribute Selector.

It's the brackets ([]) that make it an attribute selector. Angular locates each element in the template that has an attribute named appHighlight and applies the logic of this directive to that element.

After the @Directive metadata comes the directive controller class, called HighlightDirective, which contains the (current empty) logic for the directive. Exporting HighlightDirective makes the directive accessible.

Now edit the generated src/app/highlight.directive.ts look as follows

```javascript
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
    constructor(el: ElementRef) {
       el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```
The import statement specifies an additional ElementRef symbol from the Angular core library:

You use the ElementRef in the directive's constructor to inject a reference to the host DOM element, the element to which you applied appHighlight.

ElementRef grant direct access to the host DOM element through its nativeElement property.

The first implementation sets the background color of the host element to yellow.

### Apply the attribute directive
To use the new HighlightDirective, add a paragraph (&lt;p&gt;) element to the template of the root AppComponent and apply the directive as an attribute.

```html
<p appHighlight>Highlight me!</p>
```

To summarize, Angular found the appHighlight attribute on the host &lt;p&gt; element. It created an instance of the HighlightDirective class and injected a reference to the &lt;p&gt;  element into the directive constructor which sets the &lt;p&gt;  element background style to yellow.

### Respond to user-initiated events
Currently, appHighlight simply sets an element color. The directive could be more dynamic. It could detect when the mouses into or out of the element and respond by setting or clearing the highlight color.

Begin by adding HostListener to the lis of imported symbols.

```javascript
import { Directive, ElementRef, HostListener } from '@angular/core';
```

Then add two event handlers that respond when the mouse enters or leaves, each adorned by the HostListener decorator.

```javascript
@HostListener('mouseenter') onMouseEnter() {
  this.highlight('yellow');
}

@HostListener('mouseleave') onMouseLeave() {
  this.highlight(null);
}

private highlight(color: string) {
  this.el.nativeElement.style.backgroundColor = color;
}
```
The @HostListener decorator lets you subscribe to event of the DOM element that hosts an attribute directive, the &lt;p&gt; in this case.

The handlers delegate to a helper method that sets the color on the host DOM element, el.  The helper method, highlight, was extracted from the constructor. The revised constructor simply declares the injected el: ElementRef.

### Pass values into the directive with an @Input data binding
Currently the highlight color is hard-coded withing the directive. That's inflexible. 
Being by adding Input to the list of symbols imported from @angular/core.
Add a highlightColor property to the directive class like this:
```javascript
@Input() highlightColor: string;
```

***Binding to an @Input property**
Notice the @Input decorator. It adds metadata to the class that makes the directive's highlightColor property for binding. 

It's called an input property because data flows from the binding expression into the directive. Without that input metadata. Angular rejects the binding.

Try it by adding the following directive binding variations to the AppComponent template:
```html
<p appHighlight highlightColor="yellow">Highlighted in yellow</p>
<p appHighlight [highlightColor]="'orange'">Highlighted in orange</p>
```
Add a color propety to the AppComponent

```javascript
export class AppComponent {
  color = 'yellow';
}
```
Let it control the highlight color with a property binding
```html
<p appHighlight [highlightColor]="color">Highlighted with parent component's color</p>
```
That's good, but it would be nice to simultaneously apply the directive and set the color in the same attribute like this.
```html
<p [appHighlight]="color">Highlight me!</p>
```

The [appHighlight]