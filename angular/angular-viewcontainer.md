
# Component View
Each component in Angular is represented as a view with nodes. Most nodes in the view resemble component template structure and represent DOM nodes. For example, you have a component `A` with the `a-comp` selector and the following template:
```html
<h1>I am header</h1>
<span> I am {{name}}</span>
```
THe compiler generates the following view nodes:
```javascript
elementDataNode(h1);
textDataNode(I am header);
elementDataNode(span);
textDataNode(I am + bindings:[ {{name}} ]);
```

# Host View
The host view is a view that contains data for the `a-comp` component element and the data for the component class `A`. Angular compiler generates host views for each component defined in `bootstrap` or `entryComponents` of a module. And each host view is responsible for creating a component view when you call `factory.createComponent`. These factories that are returned by the `componentFactoryResolver` are the host view factories.

# Embedded View
The embedded view is a view that is created for the view nodes specified in the `ng-template`. It's like a component view but it doesn't have a wrapper component element and component data like injector etc. Basically it lacks the data that is contained in the host view. But it's still a valid view and is checking during detection as any other view.

# Exploring Angular DOM manipulation techniques using ViewContainerRef

Whenever I read about working with DOM in Angular I always see one or few of these classes mentioned: ELementRef, TemplateRef, ViewContainerRef and others. 

If you come from angular.js world, you know that it was pretty easy to manipulate the DOM. Angular injected DOM element into link function and you could query any node within component's template, add or remove child nodes, modify styles etc. However, this approach had one major shortcoming - it was tightly bound to a browser platform.

The new Angular version runs on different platforms - in a browser, on a mobile platform or inside a web worker. So a level abstraction is required to stand between platform specific API and the framework interfaces. In angular these abstractions come in a form of the following reference types: ElementRef, TemplateRef, ViewRef, ComponentRef and ViewContainerRef.

## @ViewChild
Before we explore the DOM abstraction, let's understand how we access these abstraction inside a component/directive class. Angular provides a mechanism called DOM queries. It comes in a form of @ViewChild and @ViewChildren decorators. They behave the same, only the former returns one reference, while the latter returns multiple references as QueryList object.

Usually, these decorators work in pair with **template reference variables**. A **template reference variables** is a simply a named reference to a DOM element with a template. You can view it as something similar to id attribute of an html element. You mark a DOM element with a template reference and then query it inside a class using ViewChild decorator. Here is the basic example:
```javascript
@Component({
    selector: 'sample',
    template: `
        <span #tref>I am span</span>
    `
})
export class SampleComponent implements AfterViewInit {
    @ViewChild("tref", {read: ElementRef}) tref: ElementRef;

    ngAfterViewInit(): void {
        // outputs `I am span`
        console.log(this.tref.nativeElement.textContent);
    }
}
```

In this example you can see that I specified tref as a template reference name in html and receive ElementRef associated with this element. The second parameter read is not always required, since angular can infer the reference type by the type of the DOM element. For example, if it's a simple html element like span, angular returns ElementRef. It it's a template element, it return TemplateRef. Some references, like ViewContainerRef cannot be inferred and have to be asked for specifically in read parameter. Others, like ViewRef cannot be returned from the DOM and have to be constructed manually.

## ElementRef
This is the most basic abstraction. If you observe it's class structure, you'll see that it only holds the native element it's associated with. It's useful for accessing native DOM element as we can see here:
```javascript
// outputs `I am span`
console.log(this.tref.nativeElement.textContent);
```

However , such usage is discouraged by Angular team. Not only it poses security risk, but it also creates tight coupling between your application and rendering layers which makes is diffcult to run an app on multiple platforms. I believe that it's not the access to nativeElement that breaks the abstraction, but rather usage of specific DOM API like textContent. But as you'll see later the DOM manipulation mental model implemented in Angular hardly ever requires such a lower level access.

ElementRef can be returned for any DOM element using ViewChild decorator. But since all components are hosted inside a custom DOM element and all directives are applied to DOM elements, component and directive classes can obtain an instance of ElementRef associated with their host element through DI mechanism:

```javascript
export class SampleComponent{
    constructor(private hostElement: ElementRef) {
        //outputs <sample>...</sample>
        console.log(this.hostElement.nativeElement.outerHTML);
    }
```
So while a component can get access to it's host element throught DI, the ViewChild decorator is used most often to get a reference to a DOM element in their view(template). And it's vice verse with directives - they have no views and they usually work directly  with the element they are attached to.

## Template Ref
The notion of template should be familiar for most web developers. It's a group of DOM element that are reused in views across the app. Before HTML5 standard introduced `template` tag, most templates arrived to a browser wrapper in a script tag with some variations of type attribute:
```html
<script id="tpl" type="text/template">
  <span>I am span in template</span>
</script>
```

This approach certainly had many drawbacks like the semantics and the necessity to manually create DOM models. With template tag a browser parses html and create DOM tree but not renders it. It then  can be accessed through content property:
```javascript
<script>
    let tpl = document.querySelector('tpl');
    let container = document.querySelector('tpl');
    insertAfter(container, tpl.content);
</script>
<div class="insert-after-me"></div>
<template id="tpl">
    <span>I am span in template</span>
</template>
```

Angular embraces this approach and implements. TemplateRef class to work with a template. Here is how it can be used.
```javascript
@Component({
    selector: 'sample',
    template: `
        <template #tpl>
            <span>I am span in template</span>
        </template>
    `
})
export class SampleComponent implements AfterViewInit {
    @ViewChild("tpl") tpl: TemplateRef<any>;

    ngAfterViewInit() {
        let elementRef = this.tpl.elementRef;
        // outputs `template bindings={}`
        console.log(elementRef.nativeElement.textContent);
    }
}
```
The framework removes template element from the DOM and inserts a comment in its place. This is how it look like when rendered:
```html
<sample>
    <!--template bindings={}-->
</sample>
```

By itself the TemplateRef class is a simple class. It holds a reference to its host element in the elementRef property and has one method createEmbeddedView. However, this method is very useful since it allows us to create view and return a  reference to it as ViewRef.

## ViewRef
This type of abstraction represents an angular View. In angular world a View is a fundamental building block of the application UI. It is the smallest grouping of elements which are created and destroyed together. Angular philosophy encourages developers to see UI as a composition of Views, not as tree of standalone html tags.

Angular supports two types of views:
* Embedded Views which are linked to a Template.
* Host Views which are linked to a Component.

### Creating embedded view
A template simply  holds a blueprint for view. A view can be instantiated from the template using aforementioned createEmbeddedView method like this:
```javascript
ngAfterViewInit() {
    let view = this.tpl.createEmbeddedView(null);
}
```

### Creating host view
Host views are created when a component is dynamically instantiated. A component can create dynamically using ComponentFactoryResolver:
```javascript
constructor(private injector: Injector,
            private r: ComponentFactoryResolver) {
    let factory = this.r.resolveComponentFactory(ColorComponent);
    let componentRef = factory.create(injector);
    let view = componentRef.hostView;
}
```

In Angular, each component is bound to a particular instance of an injector, so we're passing the current injector instance when creating the components that are instanitiated dynamically must be added to **EntryComponents** of module or hosting component.

So, we've seen how both embedded and host views can be created. Once a view is created it can be inserted into the DOM using ViewContainer.

## ViewContainerRef
Represent a container where one or more views can be attached.

The first thing to mention here is that any DOM element can be used as a view container. What's interesting is that Angular doesn't view inside the element, but appends them after the element bound to ViewContainer. This is similar to how router-outlet insert components.

Usually, a good candidate to mark place where a ViewContainer should be created is ng-container element. It's rendered as a comment and so it doesn't introduce redundant html elements into DOM. Here is the example of creating a ViewContainer at the specific place in a components template:
```javascript
@Component({
    selector: 'sample',
    template: `
        <span>I am first span</span>
        <ng-container #vc></ng-container>
        <span>I am last span</span>
    `
})
export class SampleComponent implements AfterViewInit {
    @ViewChild("vc", {read: ViewContainerRef}) vc: ViewContainerRef;

    ngAfterViewInit(): void {
        // outputs `template bindings={}`
        console.log(this.vc.element.nativeElement.textContent);
    }
}
```
Just as other DOM abstraction, ViewContainer is bound to a particular DOM element acccesed through element property. In the example about it's bound to ng-container element rendered as component, and so the output is template bindings = {}

### Manipulating views
ViewContainer provides a convenient API for manipulating the views:
```javascript
class ViewContainerRef {
    ...
    clear() : void
    insert(viewRef: ViewRef, index?: number) : ViewRef
    get(index: number) : ViewRef
    indexOf(viewRef: ViewRef) : number
    detach(index?: number) : ViewRef
    move(viewRef: ViewRef, currentIndex: number) : ViewRef
}
```

We've seen earilier how two types of views can be manually created from a template and a component. Once we have a view, we can insert it into a DOM using insert method. So, here is the example of creating an embedded view from a template and inserting it in a particular place marked by ng-container element:
```javascript
@Component({
    selector: 'sample',
    template: `
        <span>I am first span</span>
        <ng-container #vc></ng-container>
        <span>I am last span</span>
        <template #tpl>
            <span>I am span in template</span>
        </template>
    `
})
export class SampleComponent implements AfterViewInit {
    @ViewChild("vc", {read: ViewContainerRef}) vc: ViewContainerRef;
    @ViewChild("tpl") tpl: TemplateRef<any>;

    ngAfterViewInit() {
        let view = this.tpl.createEmbeddedView(null);
        this.vc.insert(view);
    }
}
```
With this implementation, the resulting html looks like this:
```html
<sample>
    <span>I am first span</span>
    <!--template bindings={}-->
    <span>I am span in template</span>

    <span>I am last span</span>
    <!--template bindings={}-->
</sample>
```

To remove a view from the DOM, we can use detach method. All other methods are self explanatory and can be used to get a reference to a view by the index, move the view to another location or remove all views from the container.

### Creating Views
ViewContainer also provides API to create a view automatically:
```javascript
class ViewContainerRef {
    element: ElementRef
    length: number

    createComponent(componentFactory...): ComponentRef<C>
    createEmbeddedView(templateRef...): EmbeddedViewRef<C>
    ...
}
```

These are simply convenient wrappers to what we've done manually above. They create a view from a template or component and insert it at specified location.

