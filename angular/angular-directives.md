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

The [appHighlight] attribute binding both applies the highlighting directive to the &lt;p&gt; element and sets the directive's hightlight color with a property binding. You're reuse the directive attrbitute selector ([appHightlight]) to do both jobs. That's a crisp, compact syntax.

You'll have to rename the directive's highlightColor property to appHighlight because that's now the color property binding name.

```javascript
@Input() appHighlight: string;
```

THis is disagreeable. The word, appHighlight, is a terrible property name and it doesn't convey the property's intent.

**Binding to an @Input alias**
Fortunately you can name the directive property whatever you want and alias it for binding purposes.

Restore the original property name and specify the selector as the alias in the argument to @Input.

```javascript
@Input('appHighlight') highlightColor: string;
```

Inside the directive the property is known as highlightColor. Outside the directive, where you bind to it, it's known as appHighlight.

You get the best of both worlds, the property name you want and the binding syntax you want:

```html
<p [appHighlight]="color">Highlight me!</p>
```

**Bind to a second property**
This highlight directive has a single customizable property. In a real app, it may need more. 

At the moment, the default color - the color that prevails until the user picks a highlight color - is hard coded as "red". Let the template developer set the default color.

Add a second input property to HighlightDirective called defaultColor.

```javascript
@Input() defaultColor: string;
```

Revise the directve's onMouseEnter so that it first treies to highlight with the highlightColor, then with the defaultColor, and falls back to "red" if both properties are undefined.

```javascript
@HostListener('mouseenter') onMouseEnter() {
  this.highlight(this.highlightColor || this.defaultColor || 'red');
}
```

How do you bind to a second property when you've already binding to appHighlight attrbiute name?

As with components, you can add as many directive property bindings as you need by stringing them also in the template. The developer should be able to write the following template HTML to both bind the AppComponent.color and fall back to "violet" as the default color.

```html
<p [appHighlight]="color" defaultColor="violet">
  Highlight me too!
</p>
```
Angular knowns that the defaultColor binding belongs to the HighlightDirective because you made it public with the @Input decorator.

Here's how the harness should work when you're done coding.

# Structural directives
Structural directive are responsible for HTML layout. They shape or reshape the DOM structure, typically by adding, removing  or manipulating elements.

As with other directives, you apply a structural directive to a host element. The directive then does whatever it's supposed to do that host element and its descendants.

Structural directives are easy to recognize. An asterisk(*) precedes the directive attribute name as in this example.
```html
<div *ngIf="hero" class="name">{{hero.name}}</div>
```
No brackets. No parentheses. Just *ngIf set to a string.

Angular desugars(to remove sugar from) this notation into a marked up <ng-template> that surrounds the host element and its descendants. Each structural directive does something different with that template.

Three of the common, built-in structural directives - NgIf, NgFor, and NgSwitch

# NgIf case study
NgIf is the simplest structural directive and the easiest to understand. It take a boolean expression and makes an entire chunk of the DOM appear or disappear.

```html
<p *ngIf="true">
  Expression is true and ngIf is true.
  This paragraph is in the DOM.
</p>
<p *ngIf="false">
  Expression is false and ngIf is false.
  This paragraph is not in the DOM.
</p>
```
The ngIf directive doesn't hide elements with CSS. It adds and remove them physically from the DOM. Confirm that fact using browser developer tools to inspect the DOM.

The top paragraph is in the DOM. The bottom, disused paragraph is not; in itss place is a comment about "bindings".

When the condition is false, NgIf removes it host element from the DOM, detaches it from DOM events(the attachement that it made), detaches the component from Angular change detection and destroy it. The component and DOM nodes can be garbage-collected and free up memory.

## Why remove rather than hide?
A dirtive could hide the unwanted paragraph instead of setting its display style to none.

```html
<p [style.display]="'block'">
  Expression sets display to "block".
  This paragraph is visible.
</p>
<p [style.display]="'none'">
  Expression sets display to "none".
  This paragraph is hidden but still in the DOM.
</p>
```
While invisible, the element remain in the DOM.

The difference between hiding and removing doesn't matter for a simple paragraph. It does matter when the host element is attached to a resource intensive component. Such a component's behavior continues even when hidden. The component stay attached to its DOM element. It keep listening to events. Angular keep checking for changes that could affect data binding. Whatever the component was doing, it keeps doing.

Although invisible, the component - and all of its descendant components - tie up resources. The performance and memory burden can be substantial, responsiveness can degrade and the user sees nothing.

On the positive side, showing the element again is quick. The component's previous state is preserved and ready to display. The component doesn't re-initialize - an operation that could be expensive. So hiding and showing is sometimes the right thing to do.

But in the absence of a compelling reason to keep them around, your preference should be to remove DOM elements that the user can't see and  recover the unused resources with a structural directive like NgIf.

These same consideration apply to every structural directive, whether built-in or custom. Before applying a structural directive, you might want to pause for a moment to consider the consequences of adding and removing elements and of creating and destroying components.

## The asterisk(*) prefix
Surely you noticed the asterisk(*) prefix to the directive name and wondered why it is necessary and what is does.

Here is *ngIf displaying the hero's name if hero exists.
```html
<div *ngIf="hero" class="name">{{hero.name}}</div>
```

The asterisk is "syntactic sugar" for something a bit more complicated. Internally, Angular translates the *ngIf attribute into a &lt;ng-template&gt; element, wrapped around the host element, like this.
```html
<ng-template [ngIf]="hero">
  <div class="name">{{hero.name}}</div>
</ng-template>
```
- The *ngIf directive moved to the &lt;ng-template&gt; element where it become a property binding [ngIf].
- The rest of the &lt;div&gt;, including its class attribute, move inside the &lt;ng-template&gt; element.

The first form is not actually rendered, only the finished product ends up in the DOM.

Angular consumed the &lt;ng-template&gt; content during its actual rendering and replaced the &lt;ng-template&gt; with a diagnostic comment. 

The NgFor and NgSwitch... directive follow the same pattern.

## Inside *ngFor
Angular transforms the *ngFor in similar fashion from asterisk ( *) syntax to &lt;ng-template&gt; element.

Here's a full-featured application of NgFor, written both ways:

```html
<div *ngFor="let hero of heroes; let i=index; let odd=odd; trackBy: trackById" [class.odd]="odd">
  ({{i}}) {{hero.name}}
</div>

<ng-template ngFor let-hero [ngForOf]="heroes" let-i="index" let-odd="odd" [ngForTrackBy]="trackById">
  <div [class.odd]="odd">({{i}}) {{hero.name}}</div>
</ng-template>
```

This is manifestly more complicated than ngIf and rightly so. The NgFor directive has more features, both required and optional, thant the NgIf. At mimimum needs a looping variable (let hero) and a list(heroes).

## Microsyntax
The Angular microsyntax lets you configure a directive in a compact, firendly string. The microsyntax parser translates that string into attributes on the &lt;ng-template&gt;.
- The let keyword declares a template input variable that you reference within the template. The input variables in this example are hero, i and odd. The parser translates let hero, let i and let odd into variables named let-hero, let-i and let-odd.
- The microsyntax parser title-case all directives and prefixes them with the directive's attribute's name, such as ngFor. For example, the ngFor input properties `of` and `trackBy`, become `ngForOf` and `ngForTrackBy`, respectively. That's how the directive learns that the list is heroes and the track-by functions trackById.
- As the NgFor directive loops through the list, it sets and resets properties of its own context object. These properties can include, but aren't limited to index, odd and a special property names $implicit.
- The let-i and let-odd variables were as defined as let i= index and let odd=odd. Angular set them to the current value of the context's index and odd properties.
- The context property for let-hero wasn't specified. Its intended source is implicit. Angular set let-hero to the value of the context's $implicit property, which NgFor has initialized with the hero for the current iteration.

## Writing your own structural directives
These microsyntax mechanisms are also available to you when your own structural directives. For examples, microsyntax in Angular allows you to write `<div *ngFor="let item of items">{{item}}</div>` instead of `<ng-template ngFor let-item [ngForOf]="items"><div>{{item}}</div></ng-template>`. The following sections provide detailed information on constraints. grammar, and translation of microsyntax.

### Constraints
Microsyntax must meet the following requirements:
- It must be known ahead of time so the that IDEs can parse it working knowing the underlying semantics of the directive are present.
- It must translate to key-value attribute in the DOM.

### Grammar
When you write your own structural directives, use the following grammar:

> *:prefix=(":let | :expression) (';' | ',')? ( :let | :as | :keyExp)*"

The following table describe each portion of the microsyntax grammar:

| | |
|-|-|
|prefix| HTML attribute key|
|key| HTML attribute key|
|local| local variable name used in the tempalte|
|export| value exporte by the directive under a given name|
|expression| standard Angular Expression|



| |
|-|
|keyExp = :key ":" ? expression ("as" :local)? ";"?|
|let = "let" :local "=" :export ";"?|
|as = :export "as" :local ";"?|

### Translation
A microsyntax is translated to the normal binding syntax as follows:

|Microsyntax|Translation|
|-----------|-----------|
|prefix and naked expression| [prefix]="expression"|
|keyExp | [prefixKey] "expression" (let-prefixKey="export") Notice that prefix is added to the key|
|let| let-local="export"|

## Template input variable
A template input variable whose value you can reference within a signle instance of the template. Thera are serveral such variables in this example: hero, i and odd. All thar preceed by the keyword let.

A template input-variable is not the same as a template reference variable, neither semantically nor syntactically.

You declare a template input variable using the let keyword (let here). The variable's scope is limited to a single instance of the repeated template. You can use the same variable name again in the definition of other structural directives.

You declare a template reference variable by prefixing the variable name with # (#var). A reference variable refers to its attached element, component or directive. It can be accessed anywhere in the entire template.

Template input reference variable names have their own namespaces. The hero in let hero is never the same variable as the hero declared as #hero.

## One structural directive per host element
Someday you'll want to repeat a block of HTML but only when a particular condition is true. You'll try to put both an *ngFor and an *ngIf on the same host element. Angular won't let you. You may apply only one structural directive to an element.

The reason is simplicity. Structural directives can do complex things with the host element and its descendents. When two directives lay claim to the same host element, which one take precedence?  Which should go first, the NgIf or NgFor? Can the NgIf cancel the effect of the NgFor? If so (and it seem like it should be so), how should Angular generalize the ability to cancel for other structural directives?

There are no easy answer to these questions. Prohibiting multiple structural directives make them moot(विवादास्पद). There's an easy solution for this use case: put the *ngIf on a container element that wraps the *ngFor element. One or both elements can be an ng-container so you don't have to introduce extra levels of HTML.


## Inside NgSwitch Directives
The Angular NgSwitch is actually a set of cooperating directives: NgSwitch, NgSwitchCase, and NgSwitchDefault.

Here's an example
```html
<div [ngSwitch]="hero?.emotion">
  <app-happy-hero    *ngSwitchCase="'happy'"    [hero]="hero"></app-happy-hero>
  <app-sad-hero      *ngSwitchCase="'sad'"      [hero]="hero"></app-sad-hero>
  <app-confused-hero *ngSwitchCase="'confused'" [hero]="hero"></app-confused-hero>
  <app-unknown-hero  *ngSwitchDefault           [hero]="hero"></app-unknown-hero>
</div>
```

The switch value assigned to NgSwitch(hero.emotion) determines which(if any) of the switch cases are displayed.

NgSwitch itself is not structural directive. It's an attribute that controls the behavior of the other two switched directives. That's why you write [ngSwitch], never *ngSwitch.

NgSwitchCase and NgSwitchDefault are structural directives. You attach them to elements using the asterisk(*) pefix notation. An NgSwitchCase displayd its host element when its value matched the switch value. The NgSwitchDefault displays its host element when no siblings NgSwitchCases matches the switch value.

As with other structural directives, the NgSwitchCase and NgSwitchDefault can be desugared(to remove all unnecessary syntactical elements from code) into the &lt;element&gt; element form.

```html
<div [ngSwitch]="hero?.emotion">
  <ng-template [ngSwitchCase]="'happy'">
    <app-happy-hero [hero]="hero"></app-happy-hero>
  </ng-template>
  <ng-template [ngSwitchCase]="'sad'">
    <app-sad-hero [hero]="hero"></app-sad-hero>
  </ng-template>
  <ng-template [ngSwitchCase]="'confused'">
    <app-confused-hero [hero]="hero"></app-confused-hero>
  </ng-template >
  <ng-template ngSwitchDefault>
    <app-unknown-hero [hero]="hero"></app-unknown-hero>
  </ng-template>
</div>
```

## Prefix the asterisk (*) syntax
The asterisk(*) syntax is more clear than the desugared form. Use &lt;ng-container&gt; where there's no single element to host the directive.

With there's rarely a good reason to apply a structural directive in template attribute or element form, it's stil important to know that Angular creates a &lt;ng-template&gt; and to understand how it works. 

## The &lt;ng-element&gt;
The &lt;ng-element&gt; is an Angular element for rendering HTML. It is never displayed directly. In fact, before rendering the view, Angular replaces the &lt;ng-element&gt; and its content with a comment.

If there is not structural directive and you merely wrap some elements in a &lt;ng-template&gt;, those elements disappear. That's the fate of the middle "Hip!" in the pharse "Hip! Hip! Hooray".

```html
<p>Hip!</p>
<ng-template>
  <p>Hip!</p>
</ng-template>
<p>Hooray!</p>
```
Angular erases the middle "Hip!", leaving the cheer a bit less enthusiastic.

## Group sibling elements with &lt;ng-container&gt;
There's often a root element that can and should host the structural directive. The list element (&lt;li&gt;) is a typical host element of an NgFor repeater. 

```html
<li *ngFor="let hero of heroes">{{hero.name}}</li>
```
When there isn't a host element, you can usually wrap the content in a native HTML container element, such as a &lt;div&gt;, and attach the directive to that wrapper.

```html
<div *ngIf="hero" class="name">{{hero.name}}</div>
```

Introducing another container element - typically a &lt;span&gt; or &lt;span&gt; - to group the element under a single root is usually harmless. Usually... but not always.

The grouping element may break the template appearance because CSS styles neither expect nor accommodate the new layout. For example, suppose you have the following paragraph layout.

```html
<p>
  I turned the corner
  <span *ngIf="hero">
    and saw {{hero.name}}. I waved
  </span>
  and continued on my way.
</p>
```

You also have a CSS style rule that happens to apply to a &lt;span&gt; with a &lt;p&gt;aragraph.

```css
p span { color: red; font-size: 70%; }
```
The p span style, intended for use elesewhere, was inadvertently applied here.

Another problem some HTML elements require all immediate children to be a specific type. For example, the &lt;select&gt; element requires &lt;option&gt; children. You can't wrap the options in a conditional &lt;div&gt; or a &lt;span&gt;.

When you this this,
```html
<div>
  Pick your favorite hero
  (<label><input type="checkbox" checked (change)="showSad = !showSad">show sad</label>)
</div>
<select [(ngModel)]="hero">
  <span *ngFor="let h of heroes">
    <span *ngIf="showSad || h.emotion !== 'sad'">
      <option [ngValue]="h">{{h.name}} ({{h.emotion}})</option>
    </span>
  </span>
</select>
```
the drop down is empty.

The browser won't display an &lt;option&gt; within a &lt;span&gt;

### &lt;ng-container&gt; to the rescue
The Angular &lt;ng-container&gt; is a grouping element that doesn't interfere with styles or layout because Angular doesn't put it in the DOM. 

Here's the conditional paragraph again, this time using &lt;ng-container&gt;
```html
<p>
  I turned the corner
  <ng-container *ngIf="hero">
    and saw {{hero.name}}. I waved
  </ng-container>
  and continued on my way.
</p>
```
Now condtionally exclude as select&lt;option&gt; with &lt;ng-container&gt;
```html
<div>
  Pick your favorite hero
  (<label><input type="checkbox" checked (change)="showSad = !showSad">show sad</label>)
</div>
<select [(ngModel)]="hero">
  <ng-container *ngFor="let h of heroes">
    <ng-container *ngIf="showSad || h.emotion !== 'sad'">
      <option [ngValue]="h">{{h.name}} ({{h.emotion}})</option>
    </ng-container>
  </ng-container>
</select>
```
The &lt;ng-container&gt; is a syntax element by the Angular parser. It's not a directive, component, class or interface, it's more like the curly braces in a Javascript if-block.
```javascript
if (someCondition) {
  statement1;
  statement2;
  statement3;
}
```
Without those braces, Javascript would only execute the first statement when you intend to conditionally execute all of them as a single block. The &lt;ng-container&gt; satisfies a similar need in Angular templates.

## Write a structural directive
In this section, you write an `UnlessDirective` structural directive that does the opposite of NgIf. NgIf displays the template content when the condition is true. `UnlessDirective` displays the content when the condition is false.

```html
<p *appUnless="condition">Show this sentence unless the condition is true.</p>
```

Here's how you might begin:
```javascript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({ selector: '[appUnless]'})
export class UnlessDirective {
}
```
The directive's selector is typically the directive's attribute name in square brackets, [appUnless]. The bracket define a CSS attribute selector.

The directive attribute name should be spelled in lowerCamelCase and begin with a prefix. Don't use ng. The prefix belongs to Angular. Pick something that fits you and your company. In this example, the prefix is app.

### TemplateRef and ViewContainerRef
A simple structural directive like this one create an embedded view from the Angular-generate &lt;ng-template&gt; and inserts that view in a view container adjacent to the directive original &lt;p&gt; host element.

You'll acquire the &lt;ng-template&gt; contents with a TemplateRef and access the view container through a ViewContainerRef.

You inject both in the directive constructor as private variables of the class.

```javascript
constructor(
  private templateRef: TemplateRef<any>,
  private viewContainer: ViewContainerRef) { }
```
The directive consumer expects to bind a true/false condition to [appUnless]. That means the directive needs an appUnless property, decorated with @Input.

```javascript
@Input() set appUnless(condition: boolean) {
  if (!condition && !this.hasView) {
    this.viewContainer.createEmbeddedView(this.templateRef);
    this.hasView = true;
  } else if (condition && this.hasView) {
    this.viewContainer.clear();
    this.hasView = false;
  }
}
```

Angular sets the appUnless property whenever the value of the condition changes. Because the appUnless property does work, it needs a setter.
- If the condition is falsy and the view hasn't been created previously, tell the view container to create the embedded view from the template.
- If the condition is truthy and the virw currently displayed, clear the container which also destroys the view.

Nobody reads the appUnless property so it doesn't need a getter.
The completed directive code like this:
```javascript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

/**
 * Add the template content to the DOM unless the condition is true.
 */
@Directive({ selector: '[appUnless]'})
export class UnlessDirective {
  private hasView = false;

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef) { }

  @Input() set appUnless(condition: boolean) {
    if (!condition && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (condition && this.hasView) {
      this.viewContainer.clear();
      this.hasView = false;
    }
  }
}
```
And this directive to the delcarations array of the AppModule.

Then create some HTML to try it.
```html
<p *appUnless="condition" class="unless a">
  (A) This paragraph is displayed because the condition is false.
</p>

<p *appUnless="!condition" class="unless b">
  (B) Although the condition is true,
  this paragraph is displayed because appUnless is set to false.
</p>
```

## Improving template type checking for custom directives
You can improve template type checking for custom directives by adding template guard properties to your directive definition. These properties help the Angular template type checker find mistakes in the template at compile time, which can avoid runtime errors those mistakes can cause.

Use the type-guard properties to inform the template type checker of an expected type, thus improving compile-time type-checking for that template.
- A property ngTemplateGuard_(someInputProperty) let you specify a more accurate type for an input expression within the template.
- The ngTemplateContextGuard static property declare the type of the template context.

### Make in-template type requirements more specific with template guards
A structural directive in a template controls whether that template is rendered at run time, based on its input expression. To help the compiler catch template type errors, you should specify as closely as possible the required type of a directive input expression when it occurs inside the template.

A type guard function narrows the expected type of an input expression to a subset of types that might be passed to the directive within the template at run time. You can provide such a function to help the type-checker infer the proper type for the expression at compile time.

For example, the NgIf implementation use type-narrowing to ensure that the template is only instantiated if the input expression to *ngIf is truthy. To provide the specific type requirment, the NgIf directive defines a static property ngTemplateGuard_ngIf: 'binding'. The binding value is a special case for a common kind of type-narrowing where the input expression is evaluated in order to satisfy the type requirment.

To procide a more specific type for an input expression to a directive within the template, add a ngTemplateGuard_xx property to the directibe where the suffix to the static property name is the @Input field name. The value of the property can be either a general type-narrowing function based on its return type, or the string "binding" as in the case of NgIf.

For example, consider the following structural directive that takes the result of a template expression as an input.
```javascript
export type Loaded = { type: 'loaded', data: T };
export type Loading = { type: 'loading' };
export type LoadingState = Loaded | Loading;
export class IfLoadedDirective {
    @Input('ifLoaded') set state(state: LoadingState) {}
    static ngTemplateGuard_state(dir: IfLoadedDirective, expr: LoadingState): expr is Loaded { return true; };
}

export interface Person {
  name: string;
}

@Component({
  template: `<div *ifLoaded="state">{{ state.data }}</div>`,
})
export class AppComponent {
  state: LoadingState;
}
```

In this example, the LoadingState&lt;T&gt; type permits either of two states. Loaded&lt;T&gt; or Loading. The expression used as the directive's state input is of the umberalla type LoadingState as it's known what the loading state is at that point.

The IfLoadDirective definition declares the static field ngTemplateGuard_state which expresses the narrowing behavior. Within the AppComponent template, the *ifLoaded structural directive should render this template only when state is actually Loaded&lt;Person&gt;. The type guard allows the type checker to infer that the acceptable type of state within the template is a Loaded&lt;T&gt; and further infer must be an instance of Person.

## Typing the directive's context
If your structural directive provides a context to the instantiated template, you can property type it inside the template by providing a static ngTemplateContextGuard function. The following snippet shows as example of such a function.

```javascript
@Directive({…})
export class ExampleDirective {
    // Make sure the template checker knows the type of the context with which the
    // template of this directive will be rendered
    static ngTemplateContextGuard(dir: ExampleDirective, ctx: unknown): ctx is ExampleContext { return true; };

    // …
}
```

Source: [https://angular.io/guide/structural-directives](https://angular.io/guide/structural-directives)