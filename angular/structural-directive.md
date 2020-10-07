# Template Directive Micro Syntax
(template directive micro syntax)[!assets/1_Y3T_2sJ3wOMO-Thptr0evg.png]

1. `*carousel` is the directive name. The `*` signals that we are dealing with a structural directive. Angular de-sugars this notation into `<ng-template>` that surrounds the host element and its descendants.
2. The `let` keyword declares a template a variable that you reference withing the template.
3. `from` is a binding key. You can name it however you like. Here we named it `from` to create a nice readable string `image from images`.
4. `images` is the value we want to pass into the directive.

Angular will combine the name of directive(`carousel`) with the binding key (`from`) and create a binding to an `Input`. That means that inside the directive we can refer to `images` as `@Input() carouselFrom`.
()[!assets/1_UJyQa5Z2_QptqK1fqcARDg.png]

# Domain Specific Language (DSL)
You can get very expressive with the domain specific language that you want to create inside your template directive. You aren't limited to a single binding key. Let's add two additional inputs for controlling the carousel auto-play behavior.
()[!assets/1_rGsRaI0N8TWc4SStzeIMtg.png]

# The Context Object
When we create the embedded view, we can pass in the context object. It contains all properties that we want to make available for binding in the template.
()[!assets/1_ynx33bOG-1Q5-cu386z6ww.png]

We don't know the variable name the user is going to pick while binding in the template. The user can say `let image`, `let img` or even `let item`. There is a special property on the contet object called `$implicit`. When the user says `let anyVariableName`, it actually binds it to the value specified under the `$implicit` key.

That means that this:
```html
<div *carousel="let image">
```
is equal to :
```html
<div *carousel="let image = $implicit">
```

# ViewContainerRef
The `ViewContainer` provides an API that makes changes or dynamic updates to an View Safe. It contains two types of Views: 
- **Embedded**: View created from a `TemplateRef` instance.
- **Host**: Views created using a component factory.

Any element can serve as a `ViewContainer` but it's usually implemented via `<ng-container #viewcontainer></ngcontainer>` as it is rendered as a comment and will not introduce additional DOM elements.
