# Angular Forms - what is it all about?
A large  category of frontend applications are very form-intensive, especically in the case of enterprise development. Many of these applications are basically just huge forms, spanning multiple tabs and dialogs and with non-trivial validation business logic.

Every form-intensive application has to provide answers for thr following problems:
- how to keep track of the global form state.
- know which parts of the form are valid and which are still invalid.
- properly displaying error messages to the user so that user knowns what to do to make the form valid.

Al of these are non-trivial tasks that are similar across applications, and as such could benefit from a framework.

The Angular framework provides us a couple of alternative strategies for handling forms: Let's start with the option that is the closed to Angular 1.

# Angular Template Driven forms
Angular 1 tackles form via the famous `ng-model` directive.

The instantaneous two-way data binding of `ng-model` in Angular 1 is really a life-sae as it allow to transparently keep in sync a form with a view model.

Form built with this directive can only be tested in and end to end test because this require the presence of a DOM, but still, this mechansim is very useful and simple to understand.

Angular noe provides an identical mechanism named also `ngModel`, that allow use to build what is now called Template-Driven forms.

> Note thate `NgModel` includes all of the functionality of its Angular 1 counterpart.

## Enabling Template Driven Forms
Unlike the case of Angularjs, `ngModel` and other form-related directives are not avialable by default, we need to explicitly import them in our application module:

```typescript
import {NgModule} from "@angular/core";
import {platformBrowserDynamic} from "@angular/platform-browser-dynamic";
import {BrowserModule} from "@angular/platform-browser";
import {FormsModule} from "@angular/forms";

@Component({...})
export class App { }

@NgModule({
    declarations: [App],
    imports: [BrowserModule, FormsModule],
    bootstrap: [App]
})
export class AppModule {}

platformBrowserDynamic().bootstrapModule(AppModule);
```
We can see here that we have enables Template Driven Forms by adding `FormModule` to our application, and bootstrapped the application dynamically.

### Our First Template Driven Form
Let's take a look at a form build according to the template driven way:

```html
<section class="sample-app-content">
    <h1>Template-driven Form Example:</h1>
    <form #f="ngForm" (ngSubmit)="onSubmitTemplateBased()">
        <p>
            <label>First Name:</label>
            <input type="text"  
                [(ngModel)]="user.firstName" required>
        </p>
        <p>
            <label>Password:</label>
            <input type="password"  
                [(ngModel)]="user.password" required>
        </p>
        <p>
            <button type="submit" [disabled]="!f.valid">Submit</button>
        </p>
    </form>
</section>
```
There is actually quite a lot going on in this simple example. What we have done here is to declare a simple form with two controls: first name and password, both of which are mandatory fields(marked with the `required` attribute).

The form will trigger the controller method `onSubmitTemplateBased` on submission, but the submit button is only enabled if both required field are filled in.

### NgModel Validation Functionality
Notice the use of [(ngModel)], this notation emphasizes that the two form controls are bi-directionally bound with a view model variable, named as simply `user`.

> This [(ngModel)] syntax is knows as the 'Box of Bananas' syntax :-)

More than that, when the user click a required field, the field is show in red until the user type in something. Angular is actually tracking three form field state for us and applying the following CSS classes to both the form and its control:
- touched and untouched
- valid and invalid
- pristine or dirty

These CSS state classes are very useful for styling form error states.

Angular is actually tracking the validity state of the whole form as well, using it to enable/disable the submit button. This functionality is actually common to both template-driven and reactive forms.

#### The logic for all this must be in the controller, right?
Let's take a look at the controller associated with this view to see how all this form logic is implemented:

```typescript
@Component({
    selector: "template-driven-form",
    templateUrl: 'template-driven-form.html'
})
export class TemplateDrivenForm {

    user: Object = {};

    onSubmitTemplateBased() {
        console.log(this.vm);
    }

}
```
Not much to see here! We only have a declaration for a view model object `user`, and an event handler used bt `ngSubmit`.

All the very useful functionality of tracking form errors and registering validators is taken care for us without any special configuration.

### How does Angular pull this off then?
The way that this work, is that there is a set of implicitly defined form directive that are being applied to the view. Angular will automatically apply a form-level directive to the form in a transparent way, creating a `FormGroup` and linking it to the form.

If by some reason you don't want this you can always disable this functionality by adding `ngNoForm` as a form attribute.

Furthermore, each input will also get applied a directibe that will register itself with the control group, and validators are registered if like elements like `required` or `maxLength` are applied to the input.

The presence of [(ngModel)] will also create a bidirectional binding between the form and the user model, so in the end there is not much more to do at the level of the controller.

This is why this called template-driven forms, because both validation and binding are all setup in a declarative way at the level of the template.

#### What if we don't need bi-directional binding, but only field initialization ?
Sometimes we just want to create a form and initialize it, but not necessarily do bi-directional binding. We could want to let the user fill in the form and press submit and only then get the latest value. We can do this by using the plain [ngModel] syntax:

```html
<p>
    <label>First Name:</label>
    <input type="text"  [ngModel]="user.firstName" required>
</p>
<p>
    <label>Password:</label>
    <input type="password"  [ngModel]="user.password" required>
</p>
```

This will allow to initalize the form by filling in the fields of the user object:

```javascript

user = {
    firstName: 'John',
    password:  'test'
};
```
#### What if we don't need field initialization, can we still get validation ?
For example, creation form don't need initial values, they only need validation. If we want to go only the validation functionality of `ngModel` without neither the initialization if values or the bi-directional binding, we can do so with the following syntax:

```html
<p>
      <label>First Name:</label>
      <input type="text" name="firstName" required ngModel>
</p>
<p>
      <label>Password:</label>
      <input type="password" name="password" required ngModel>
</p>
```
### Advantages and Disdavantages of Template Driven forms
In this simple example we cannot really see it, but keeping the template as the source of all form validation truth is something that can become pretty hard to read rather quickly.

As we add more and more validator tags to a field or when we start adding complex cross-field validations the readability of the form decreases, to the point where it will be harder to hand it off to a web designer.

The upside of this way of handling forms is its simplicity, and it's probably more than enough to build a large ranger of forms.

On the downside, the form validation logic cannot be unit tested. The only way to test this logic is to run an end to end test with a browser, for example using a headless browser like `PhantomJS`

### Template Driven Forms from a funcational programming point of view
There is nothing wrong with template driven forms, but from a programming technique point of view bi-directional bindinf is a solution that promates mutability.

Each form has a state that can be updated by many different interactions and it's up to the application developer to manage that state and prevent it from getting corrupted. This can get hard to do very large forms and can introduce a category of potential bugs.

Again it's important to realize that this only happens in very large/complex forms. Angular does provide a different alternative for managing forms, so let's go through it.

# Reactive Forms(or Model Driven)
A reative form looks on the surface pretty much like a template driven form. But in order to be able to create this type of forms, we need first to import a different module into our application:

```typescript
import {NgModule} from "@angular/core";
import {ReactiveFormsModule} from "@angular/forms";

@Component({...})
export class App { }

@NgModule({
    declarations: [App],
    imports: [BrowserModule, ReactiveFormsModule],
    bootstrap: [App]
})
export class AppModule {}
```
Note that here we imported `ReactiveFormsModule` instead of `FormModule`. This will load the reactive forms directives instead of the template driven directives.

## Our First Reactive Form
Let's take our previous example and re-write it but this time in reactive style:
```html
<section class="sample-app-content">
    <h1>Model-based Form Example:</h1>
    <form [formGroup]="form" (ngSubmit)="onSubmit()">
        <p>
            <label>First Name:</label>
            <input type="text" formControlName="firstName">
        </p>
        <p>
            <label>Password:</label>
            <input type="password" formControlName="password">
        </p>
        <p>
            <button type="submit" [disabled]="!form.valid">Submit</button>
        </p>
    </form>
</section>
```
There are a couple of differeneces here. First, there is a `formGroup` directive applied to the whole form, binding it to controller variable names `form`.

Notice also that the `required` validator attribute is not applied to the form controls. This means the validation logic must be somewhere in the controller, where it can be unit tested.

### What does the controller look like?
There  is a bit more going on in the controller of a Reactive Form, let's take a look at the controller for the form above:

```typescript
import { FormGroup, FormControl, Validators, FormBuilder } 
    from '@angular/forms';

@Component({
    selector: "model-driven-form",
    templateUrl: 'model-driven-form.html'
})
export class ModelDrivenForm {
    form: FormGroup;
    
    firstName = new FormControl("", Validators.required);
    
    constructor(fb: FormBuilder) {
        this.form = fb.group({
            "firstName": this.firstName,
            "password":["", Validators.required]
        });
    }
    onSubmitModelBased() {
        console.log("model-based form submitted");
        console.log(this.form);
    }
}
```
We can see that the form is really just a `FormControl`, which keeps track of the global validity state. The controls themselves can either be instantiated individually or defined using  a simplied array notation using the form builder.

In the array notation, the first element of the array is the initial value of the control, and the remaining elements are the control's validators. In this case both controls are made mandatory via the `Validators.required` built-in validator.

### But what happened to `ngModel`?
Note that `ngModel` can still be used with reactive forms. It is just that the form value would be available in two different places : the view model and the `FormGroup`, which could potentially lead to some confusion.

Due to this reason mixing `ngModel` with reactive forms is best avoided.

### Advantages and disadvantages of Reactive Forms
You are probably wondering what we gained here. On the surface there is already a big gain: We can now unit test the form validation logic.

We can do that just by instantiating the class, settins some values in the form controls and perform assertions against the form global valid state and the validity state of each control.

But this is just one possibility. The `FormGroup` and `FormControl` classes provide an API that allows us to build UIs using completely different programming style unknown as Functional Reactive Programming.

### Functional Reactive Programming in Angular
The form controls and the form itself provides an Observable based API. You can think of observable simply as a collection of values over time.

This means that both the controls and the whole form itself can be viewed as continous stream of values, that can be subscribed to and processed using commonly used functional primitives.

For example, it's possible to subscribe to the Form stream of values using the Observabe API like this: 
```typescript
this.form.valueChanges
    .map((value) => {
        value.firstName = value.firstName.toUpperCase();
        return value;
    })
    .filter((value) => this.form.valid)
    .subscribe((value) => {
        console.log("Model Driven Form valid value: vm = ",
                    JSON.stringify(value));
    });   
```
What we are doing here is taking the stream of form values(that changes each time the user in an input field), and then apply to it some commonly used functional programming operators: `map` and `filter`.

In fact, the form stream provides the whole range of functional operators available in `Array` and many more.

In this case, we are converting the first name to uppercase using `map` and taking only the valid form values using `filter`. This create a new stream of **valid-only** values to which we subscribe, by providing a callback that defines how the UI should react to a new valid value.

### Advantages of building UIs using Functional Reactive Programming (FRP)
We are not obliged to use FRP techniques with Angular Reactive Forms. Simply using them to make the templates cleaner and allow for component unit testing is already a big plus.

But like it usual in the case of Observable-based APIs, FRP techniques can help easily implementation many  use cases that would otherwise be rather hard to implment such as:
- pre-save the form in the background at each valid state or even invalid (for example storing the invalid value in a cookie for later use).
- typical desktop features like undo/redo

### Updating Form Values
We now have APIs available for either updating the whole form, or just a couple of fields. For example, let's create a couple of new buttons on the reactive form above:
```html
<p>
    <button type="submit" [disabled]="!form.valid">Submit</button>
    <button (click)="partialUpdate()">Partial Update</button>
    <button (click)="fullUpdate()">Full Update</button>
    <button (click)="reset()">Cancel</button>
</p>
```
We can see here that there are two buttons for updating the form value, one for the partial updates and other for full updates. This is how the corresponding component methods look like:
```typescript
fullUpdate() {
    this.form.setValue({firstName: 'Partial', password: 'monkey'});
}

partialUpdate() {
    this.form.patchValue({firstName: 'Partial'});
}
```
We can see that `FormGroup` provides two API method for updating form values:
- we have `patchValue()` which partially updates the form. This method does not need to receive values for all fields of the form, which allows for partial updates.
- there is also `setValues()`, to which we are passing all the values of the form. In the case of this method, values for all form fields need to be provided, otherwise, we will get an error message saying that certain field values are missing.

We might think that we could use these same APIs to reset the form by passing blank values to all fields.

That would not work as intended because the pristine and untouched statuses of the form and its field would not get reset according. But Angular Final provides an API for this use case.

#### How to Reset a Form
Notice on the button list above that there is a cancel button that call a `reset()` method, which does reset everything back to pristine and untouched:
```typescript
reset() {
    this.form.reset();
}
```

## Reactive vs Template Driven: can they ve mixed ?
Reactive and Template - Driven under the hood are implemented in the same way: there is a `FormGroup` for the whole form and one `FormControl` instance per each individual control.

If by some reason we would need to, we could mix and match the two ays of building forms. for example: 
- We can use `ngModel` to read the data and use `FormBuilder` for the validations we don't have to subscribe to the form or use RxJS if we don't wish to.
- We can declare a control in the controller, and then reference it in the template to obtain its validity state.

But in general, it's better to choose one of the two ways of doing forms, and using it consistently throughout the application.

## Which form type to choose, and why ?
Template Driven forms are maybe for simple forms slightly less verbose, but the difference is not significant. Reactive Forms are actually much more powerful and have a nearly equivalent readability.

Most likely in a large-scale application, we will end up the functionality of reactive driven forms for implementing more advanced use case like for example auto-save.

### Everthing can be done in both form types, but some things are simpler using reactive forms
It's not this that there is functionality that cannot be implemented with template driven forms. But there is a lot of functionality especially more modern form features like for example auto-save that can be implemented in just a few line of RxJs.

### Which form type to choose ?
Are you migrating an Angular 1 application into Angular? That is the ideal scenario for using Template Driven Forms.

Or are you building a new application from scratch? Reactive forms are a good default choice because more complex validation logic is actually simpler to implement using them.

For example, imagine a validation that requires to inspect two fields and compare them: for example a password field and a password confirmation field need to be identical.

With reactive forms, we just need to write a function and plug it into the `FormControl`. With template driven forms, there is more to it: we need to define a directive and somehow pass it the values of the two fields.

Reactive forms seem great for example for enterprise applicationd with lots of complex inter-field business validation logic.

As mentioned before, we want to avoid situations where we are using both form types together, as it can get rather confusing. But it's still possible to use both forms together if by some reason we really need to.



