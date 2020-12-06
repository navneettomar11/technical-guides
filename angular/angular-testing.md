# Angular Unit Testing

## Jasmine
Jasmine is a javascript testing framework that supports a software development practice called Behavior Driven Development, or BDD for short. It’s a specific flavor of Test Driven Development(TDD).
Jasmine, and BDD in general, attempts to describe tests in a human readable format so that non-technical people can understand what is being tested. However even if you are technical reading tests in BDD format makes it a lot easier to understand what’s going on.
For example if we wanted to test this function:
```javascript
function helloWorld(){
	return “Hello World”;
}
```
We would write a jasmine test spec like so:
```javascript
describe(‘Hello world’, () => { //1
	it(’says shello’, () => { //2
		expect(helloWorld()). //3
			toEqual(‘Hello World’); //4
	});
});
```

1. The **describe(string, function)** function defines what we call a Test suite, a collection of individual Test specs.
2. The it(string, function) function defines an individual Test spec, this contains one or more Test Expectations.
3. The **expect(actual)** expression is what we call an Exception. In conjunction with a Matcher it describe an expected piece of behavior in the application. 
4. The **matcher(expected)** expression is what we call a Matcher. It does comparison with the expected value passed in vs the actual value passed to the expect function, if they are false the spec fails.

## Built-in matchers
Jasmine come with a few pre-built matchers like so:
* expect(array).toContain(member);
* expect(fn).toThrow(string);
* expect(fn).toThrowError(string);
* expect(instance).toBe(instance);
* expect(mixed).toBeDefined();
* expect(mixed).toBeFalsy();
* expect(mixed).toBeNull();
* expect(mixed).toBeTruthy();
* expect(mixed).toBeUndefined();
* expect(mixed).toEqual(mixed);
* expect(mixed).toMatch(pattern);
* expect(number).toBeCloseTo(number, decimalPlaces);
* expect(number).toBeGreaterThan(number);
* expect(number).toBeLessThan(number);
* expect(number).toBeNaN();
* expect(spy).toHaveBeenCalled();
* expect(spy).toHaveBeenCalledTimes(number);
* expect(spy).toHaveBeenCalledWith(...arguments);

## Setup and teardown
Sometimes in order to test a feature we need to perform some setup, perhaps it’s creating some test objects. Also we may need to perform some cleanup activities after we have finished testing, perhaps we need to delete some files from the hard drive.
These activities are called setup and teardown(for cleaning up) and Jasmine has a few functions we can use to make this easier”
**beforeAll**: This function is called one, before all the specs in describe test suite are finished.
**afterAll**: This function is called once after all the specs in a test suite are finished
**beforeEach**: This function is called before each test specification, it function has been run.
**afterEach**: This function is called after each test specification has been run.

We might use these function like so:
```javascript
describe(‘Hello world’, () => {
	let expected = “”;
	beforeEach(() =>{
		expected = “Hello World”;
	});
	afterEach(()=>{
		expected = “”;
	});
	it(‘say hello’, () =>{
		expect(helloWorld()).toEqual(expected);
	});
});
```

## Karma
Manually running Jasmine tests by refreshing a browser tab repeatedly in different browsers every-time we edit some code can become tiresome.

Karma is a tool which lets us spawn browsers and run jasmine tests inside of them all from the command line. The results of the tests are also displayed on the command line.

Karma can also watch your development files for changes and re-run the tests automatically.

Karma let us run jasmine tests as part of a development tool chain which requires tests to be runnable and results inspectable via the command line.

It’s not necessary to know the internals of how Karma works. When using the Angular CLI it handles the configuration for us and the rest of this section we are going to run the tests using only Jasmine.

## Angular CLI
When creating Angular projects using the Angular CLI it defaults to creating and running unit tests using Jasmine and Karma.

Whenever we create files using the CLI as well as creating the main code file it also creates simple jasmine spec file name the same as the main code file but ending in .spec.ts; like so:

If we create  a Pipe using the CLI like so:
> ng generate pipe My

This would create two files:
* my-pipe.ts - This is the main code file where we put the code for the pipe.
* my-pipe.spec.ts - This is the jasmine test suite for the pipe.

The spec file will have some code already bootstrapped, like so:
> /* tslint:disable:no-unused-variable */

```javascript
import { TestBed, async } from '@angular/core/testing';
import { MyPipe } from './my.pipe';

describe('Pipe: My', () => {
  it('create an instance', () => {
    let pipe = new MyPipe();
    expect(pipe).toBeTruthy();
  });
});
```
To run all the tests in our application we simply type `ng test` in our project root.
This run all the tests in our project in Jasmine via Karma.

It watches for changes to our development files, bundles all the developer files together and re-run the test automatically.

## Testing Classes & Pipes
We’ll start our unit testing journey with all you ever need to know, how to test a class.
Tip: Everything in Angular is an instance of a class, be it a Component directive, pipe and so on. So once you know how to test a basic class you can test everything.

Let’s imagine we have a simple class called AuthService it’s something we want to provide to Angulars DI framework but that doesn’t play a party in how we want to test it.
```javascript
export class AuthService{
	isAuthenticated(): boolean {
		return !!localStorage.getItem(’token’);
	}
}
```
It has one function called isAuthenticated with returns true if there is a token stored in the browsers localStorage
To test this class we create a test file called auth.service.spec.ts that sits next to our auth.service.ts

```javascript
import {AuthService} from ‘./auth.service’;  //1
describe(‘service auth’, ()=>{.  //2
});
```
1. We first import the AuthService class we want to run our tests against.
2. We add a describe test suite function to hold all our individual test specs.

### Setup & teardown
We want to run out test specs against fresh instances of AuthService so we use the beforeEach and afterEach functions to setup and clean instance like so:
```javascript
describe(’service auth’, () =>{
	let service:AuthService;
	
	beforeEach(() =>{ //1
		service = new AuthService();
	});
	
	afterEach(()=>{. //2
		service  = null; 
		localStorage.removeItem(‘token’);
	});
});
```
1. Before each test spec is run we create a new instance of AuthService and store on the service variable.
2. After each test spec is finished we null out our service and also remove any tokens we stored in localStorage.

### Creating test specs
Now we create some test specs, the first spec I want to create should check if the isAuthenticated function returns true when there is a token.
```javascript
it(’should return true from is Authenticated when there is a token’ , () => { //1
	localStorage.setItem(‘token’,’1234’);   //2
	expect(service.isAuthenticated()).toBeTruthy();  //3
});
```
1. We pass to the it function a human readable description of what we are testing. This is shown in the test report and make it easy to understand what feature isn’t working.
2. We setup some spec only data in localStorage which should trigger the effect we want.
3. We test an expectation that the service.isAuthenticated() function returns something that resolve to true.

We also want to the test the reverse case, when there is no token the function should return false;
```javascript
it(’should return false from isAuthenticated when there is no token’, () =>{
	expect(service.isAuthenticated()).toBeFalsy();
});
```
We know that in this function the token isn’t set since we make sure to clear out the token in the afterEach function.
We now test an expectation that the service.isAuthenticated() function returns something that resolves to false.

## Pipes
Pipes are by far the simplest part of Angular, they can be implemented as a class with one function and therefore can be tested with just jasmine and the knowledge we’ve gained so far.
In this section on pipes we built one called DefaultPipe, this pipe let us provide default values for variables in templates like so:
```javascript
{{ image | default:”http://example.com/default-image.png”}}
```
The code for this pipe looked like so:
```javascript
import {Pipe, PipeTransform} from ‘@angular/core’;

@Pipe({
	name: ‘default
})
export class DefaultPipe implements PipeTransform{
	
transform(value:string, fallback:string, forceHttps:boolean = false): string {
	let image = “”;
	if(value){
		image = value;
	}else {
		image = fallback;
	}

	if(forceHttps){
		if(image.indexOf(“https”) === -1){
			image = image.replace(“http”, “https”);
		}
	}
	return image;
}
```
Our starting test suite file look like so:
```javascript
describe(‘Pipe: Default’, ()=>{
	let pipe: DefaultPipe;
	
	beforeEach(() => {
		pipe = new DefaultPipe();
	});
});
```
In our setup function we create an instance of our pipe class.

Pipe classes have one function called transform so in order to test pipes we just need to test this one function, passing inputs and expecting outputs.

Our first test spec checks to see that if the pipe doesn’t receive an input it return the default value like so:
```javascript
it(‘providing no value return fallback’,()=>{
	expect(pipe.transform(‘’, ‘http://place-hold.it/300')).toBe(‘http://place-hold.it');
});
```
We pass in empty string as the input to the transform function and therefore it returns the second argument back to us.
For testing pipes there isn’t much else to it, we simply check the various inputs and expected outputs of our transform function.

Testing with Mock & Spies
Let’s imagine we have a LoginComponent which work with the AuthService we tested in the previous lecture, like so:

import {Component} from '@angular/core';
import {AuthService} from "./auth.service";

@Component({
  selector: 'app-login',
  template: `<a [hidden]="needsLogin()">Login</a>`
})
export class LoginComponent {

  constructor(private auth: AuthService) {
  }

  needsLogin() {
    return !this.auth.isAuthenticated();
  }
}

We inject the AuthService into the LoginComponent and the component shows a Login button the AuthService says the user isn’t authenticated.

export class AuthService {
  isAuthenticated(): boolean {
    return !!localStorage.getItem('token');
  }
}

Testing with the real AuthService
We could test the LoginComponent by using a real instance of AuthService but if you remember to returning true for the isAuthenticated function we needed to setup some data via localStorage.
import {LoginComponent} from './login.component';
import {AuthService} from "./auth.service";

describe('Component: Login', () => {

  let component: LoginComponent;
  let service: AuthService;

  beforeEach(() => {  //1
    service = new AuthService();
    component = new LoginComponent(service);
  });

  afterEach(() => {  //2
    localStorage.removeItem('token');
    service = null;
    component = null;
  });


  it('canLogin returns false when the user is not authenticated', () => {
    expect(component.needsLogin()).toBeTruthy();
  });

  it('canLogin returns false when the user is not authenticated', () => {
    localStorage.setItem('token', '12345');  //3
    expect(component.needsLogin()).toBeFalsy();
  });
});

1. We create an instance of AuthService and inject it into out LoginComponent when we create it.
2. We clean up data and local storage after each test spec has been run.
3. We setup some data in localStorage in order to get the behavior  we want from AuthService.
So in order to test LoginComponent we would need to know the inner workings of AuthService.

That’s not very isolated but also not too much to ask for in this scenario. However imagine if LoginComponent required a number of other classes just to test our LoginComponent.

This results in Tight Coupling and our tests being very Brittle I.e. likely to break easily. For example if the AuthService change how it stored the token, from localStorage to cookies then the LoginComponent test would break since it would still be setting the token via localStorage.
This is why we need to test classes in isolation, we just want to worry about LoginComponent and not about the myriad of other things LoginComponent depends on.
We achieve this by Mocking our dependencies. Mocking is the act of creating something that look like the dependency but is something we control in our test. There are a few methods we can use to create mocks.

Mocking with fake classes
We can create a fake AuthService called MockedAuthService which just returns whatever we want for our test.
We can even remove the AuthService import if we want there really is no dependency on anything else. The LoginComponent is tested isolation.
import {LoginComponent} from './login.component';

class MockAuthService {  //1
  authenticated = false;

  isAuthenticated() {
    return this.authenticated;
  }
}

describe('Component: Login', () => {

  let component: LoginComponent;
  let service: MockAuthService;

  beforeEach(() => {  //2
    service = new MockAuthService();
    component = new LoginComponent(service);
  });

  afterEach(() => {
    service = null;
    component = null;
  });


  it('canLogin returns false when the user is not authenticated', () => {
    service.authenticated = false;  //3
    expect(component.needsLogin()).toBeTruthy();
  });

  it('canLogin returns false when the user is not authenticated', () => {
    service.authenticated = true;  //3
    expect(component.needsLogin()).toBeFalsy();
  });
});

1. We create a class called MockAuthService which has the same isAuthenticated function as the real AuthService class. The one difference is that we can control what isAuthenticated returns by setting the value of the authenticated property.
2. We inject into our LoginComponent an instance of the MockAuthService instead of the real AuthService.
3. In our tests we trigger the behavior we want from the service by setting the authenticated property.

By using a fake MockAuthService we:
* Don’t depend on the real AuthService, in fact we don’t even need to import it into specs.
* Make our code less brittle, if the inner workings of the real AuthService ever changes our test will still be valid and still work.

Mocking by override functions
Sometimes creating a complete fake copy of a real class can be complicated, time consuming and unnecessary.
We can instead simply extend the class and override one or more specific function in order to get them to return the test response we need, like so:
class MockAuthService extends AuthService {
  authenticated = false;

  isAuthenticated() {
    return this.authenticated;
  }
}

In the above class MockAuthService extends AuthService. It would have access to all the other functions and properties that exist on AuthService but only override the isAuthenticated function so we can easily control it’s behavior and isolate our LoginComponent test.

Mocking by using a real instance with Spy
A Spy is a feature of Jasmine which lets you take an existing class, function, object and mock it in such a way that you can control what gets returned from functions.
Let’s re-write our test to use a Spy on a real instance of AuthService instead, like so:
import {LoginComponent} from './login.component';
import {AuthService} from "./auth.service";

describe('Component: Login', () => {

  let component: LoginComponent;
  let service: AuthService;
  let spy: any;

  beforeEach(() => {   //1
    service = new AuthService();
    component = new LoginComponent(service);
  });

  afterEach(() => {  //2
    service = null;
    component = null;
  });


  it('canLogin returns false when the user is not authenticated', () => {
    spy = spyOn(service, 'isAuthenticated').and.returnValue(false);  //3
    expect(component.needsLogin()).toBeTruthy();
    expect(service.isAuthenticated).toHaveBeenCalled();  //4

  });

  it('canLogin returns false when the user is not authenticated', () => {
    spy = spyOn(service, 'isAuthenticated').and.returnValue(true);
    expect(component.needsLogin()).toBeFalsy();
    expect(service.isAuthenticated).toHaveBeenCalled();
  });
});

1. We create a real instance of AuthService and inject it into the LoginComponent.
2. In our teardown function there is no need to delete the token from localStorage.
3. We create a spy on our service so that if the isAuthenticated function is called returns false.
4. We can even check to see if the isAuthenticated function was called.

By using the spy feature of jasmine we can make any function return anything we want:
spyOn(service, 'isAuthenticated').and.returnValue(false);

Angular Test Bed
The Angular Test Bed(ATB) is a higher level Angular Only testing framework that allows us to easily test behaviors that depend on the Angular Framework.
Let demonstrate how to use the ATB by converting the component we tested with plain vanilla Jasmine to one use the ATB.

/* tslint:disable:no-unused-variable */
import {TestBed, ComponentFixture} from '@angular/core/testing';
import {LoginComponent} from './login.component';
import {AuthService} from "./auth.service";

describe('Component: Login', () => {

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [LoginComponent],
      providers: [AuthService]
    });
  });
});

In the beforeEach function for our test suite we configure a testing module using the TestBed class.

This create a test Angular module which can use to instantiate components, perform dependency injections and so on.

We configure it in exactly the same way as we would configure a normal NgModule. On this case we pass in the LoginComponent in the declarations and the AuthService in the providers.

Fixtures and DI
Once the ATB is setup we can then use it to instantiate components and resolve dependencies, like so:

/* tslint:disable:no-unused-variable */
import {TestBed, ComponentFixture} from '@angular/core/testing';
import {LoginComponent} from './login.component';
import {AuthService} from "./auth.service";

describe('Component: Login', () => {

  let component: LoginComponent;
  let fixture: ComponentFixture<LoginComponent>;    //1
  let authService: AuthService;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [LoginComponent],
      providers: [AuthService]
    });

    // create component and test fixture
    fixture = TestBed.createComponent(LoginComponent);    //2

    // get test component from the fixture
    component = fixture.componentInstance;  //3

    // UserService provided to the TestBed
    authService = TestBed.get(AuthService);  //4

  });
});

1. A fixture is a wrapper for a component and it’s template.
2. We create an instance of a component fixture through the TestBed, this injects the AuthService into the component constructor.
3. We can find the actual component from the componentInstance on the fixture.
4. We can get resolve dependencies using the TestBed injector by using the get function.

Test specs
Now we’ve configured the TestBed and extracted the component and service we can run through the same test specs as before:
it('canLogin returns false when the user is not authenticated', () => {
  spyOn(authService, 'isAuthenticated').and.returnValue(false);
  expect(component.needsLogin()).toBeTruthy();
  expect(authService.isAuthenticated).toHaveBeenCalled();
});

it('canLogin returns false when the user is not authenticated', () => {
  spyOn(authService, 'isAuthenticated').and.returnValue(true);
  expect(component.needsLogin()).toBeFalsy();
  expect(authService.isAuthenticated).toHaveBeenCalled();
});

When to use ATB
1. It allows us to test the interaction of a directive or component with it’s template.
2. It allows us to easily test change detection.
3. It allows us to test and use Angular DI framework.
4. It allows us to test using the NgModule configuration we use in our application.
5. It allows us to test user interaction via clicks & input fields.

Testing Change Detection
Trying to test whether changes in the state of our application trigger changes in the view without the Angular Test Bed is complicated. However with the ATB it’s much simpler.

We’’ continue testing our LoginComponent but this time we’ll update the template so we have both a Login and Logout button like so:
@Component({
  selector: 'app-login',
  template: `
  <a>
    <span *ngIf="needsLogin()">Login</span>
    <span *ngIf="!needsLogin()">Logout</span>
  </a>
`
})
export class LoginComponent {

  constructor(private auth: AuthService) {
  }

  needsLogin() {
    return !this.auth.isAuthenticated();
  }
}

Our test spec file starts close to the version we have in the last lecture like so:
/* tslint:disable:no-unused-variable */
import {TestBed, async, ComponentFixture} from '@angular/core/testing';
import {LoginComponent} from './login.component';
import {AuthService} from "./auth.service";
import {DebugElement} from "@angular/core";   //1
import {By} from "@angular/platform-browser";  //1

describe('Component: Login', () => {

  let component: LoginComponent;
  let fixture: ComponentFixture<LoginComponent>;
  let authService: AuthService;
  let el: DebugElement;  //2

  beforeEach(() => {

    // refine the test module by declaring the test component
    TestBed.configureTestingModule({
      declarations: [LoginComponent],
      providers: [AuthService]
    });

    // create component and test fixture
    fixture = TestBed.createComponent(LoginComponent);

    // get test component from the fixture
    component = fixture.componentInstance;

    // UserService provided to the TestBed
    authService = TestBed.get(AuthService);

    //  get the "a" element by CSS selector (e.g., by class name)
    el = fixture.debugElement.query(By.css('a'));  //3
  });
});

1. We’ve imported a few more classes that are needed when interacting with a components view, DebugElement and By.
2. We have another variable called el which holds something called a DebugElement.
3. We store a referent to a DOM element in our el variable.

The fixture as well as holding an instance of the component also holds a reference to something called a DebugElement, this a wrapper to the low level DOM element that represents the component view, via the debugElement property.

We can get references to other child nodes by querying this debugElement with a By class. The By class let us query using a number of methods, one is via a css class like we have in our example another way is to request by a type of directive like By.directive(MyDirective).

We request a reference to the a tag that  exists in the components view, this is the button which either says Login or Logout depending on whether the AuthService says the user is authenticated or not.

We can find out the text content of the tag by calling el.nativeElement.textContent.trim(), we’ll be using that snippet in the test specs later on.

Lets now add a basic test spec like so:
it('login button hidden when the user is authenticated', () => {
  // TODO
});

Detect Changes
The first expectation we  place in our test spec might look a bit strange
it('login button hidden when the user is authenticated', () => {
  expect(el.nativeElement.textContent.trim()).toBe('');
});

We initially expect the text inside the a tag to be blank.

That’s because when Angular first loads no change detection has been triggered and therefore the view doesn’t show either the Login or Logout text.

Fixture is a wrapper for our components environment so we can control  things like change detection.

To trigger change detection we call the function fixture.detectChanges(), now we can update our test spec to:
it('login button hidden when the user is authenticated', () => {
  expect(el.nativeElement.textContent.trim()).toBe('');
  fixture.detectChanges();
  expect(el.nativeElement.textContent.trim()).toBe('Login');
});

Once we trigger a change detection run Angular checks property bindings and since the AuthService defaults to not authenticated we show the text Login.

Now lets change the AuthService so it now returns Authenticated, like so:
it('login button hidden when the user is authenticated', () => {
  expect(el.nativeElement.textContent.trim()).toBe('');
  fixture.detectChanges();
  expect(el.nativeElement.textContent.trim()).toBe('Login');
  spyOn(authService, 'isAuthenticated').and.returnValue(true);
  expect(el.nativeElement.textContent.trim()).toBe('Login');
});
But at this point the button content still isn’t Logout, we need to trigger another change detection run like so:
it('login button hidden when the user is authenticated', () => {
  expect(el.nativeElement.textContent.trim()).toBe('');
  fixture.detectChanges();
  expect(el.nativeElement.textContent.trim()).toBe('Login');
  spyOn(authService, 'isAuthenticated').and.returnValue(true);
  expect(el.nativeElement.textContent.trim()).toBe('Login');
  fixture.detectChanges();
  expect(el.nativeElement.textContent.trim()).toBe('Logout');
});

Now we’ve triggered a second change detection run Angular detected that the AuthService return true and the button test updated to Logout accordingly.

Testing Asynchronous Code
So we change out AuthService.isAuthenticated() function to an asynchronous one that return a promise which resolves into a boolean at a later time.

export class AuthService {
  isAuthenticated(): Promise<boolean> {
    return Promise.resolve(!!localStorage.getItem('token'));
  }
}

We also then change our LoginComponent:
export class LoginComponent implements  OnInit {

  needsLogin: boolean = true;

  constructor(private auth: AuthService) {
  }

  ngOnInit()  {
    this.auth.isAuthenticated().then((authenticated) => {
      this.needsLogin = !authenticated;
    })
  }
}

We’ve changed needsLogin from a function into a property and we set the value of this property in the then callback from the promise returned from AuthService.

Importantly we’ve done the above in the ngOnInit() lifecycle function. Probably not the best place to put this functionality given that the value might change over time but good for demonstration purposes.

No asynchronous handling
Our first attempt might to be try to test our application without taking into account the asynchronous nature of our app like so:
  it('Button label via jasmine.done', () => {
    fixture.detectChanges();  //1
    expect(el.nativeElement.textContent.trim()).toBe('Login');  //2
    spyOn(authService, 'isAuthenticated').and.returnValue(Promise.resolve(true));  //3
    component.ngOnInit();  //4
    fixture.detectChanges();  //5
    expect(el.nativeElement.textContent.trim()).toBe('Logout');  //6
  });

1. We issue our first change detection run so the view does it initial update.
2. We expect the button test to display Login
3. We change our AuthService so it returns a promise resolved to true.
4. We call component.ngOnInit()
5. We issue our second change detection run.
6. We now expect the button text to read Logout.

Important : When performing test we need to call component lifecycle hook ourselves, like ngOnInit(). Angular won’t do this for us in the test environment.

If we ran the above code we would see it doesn’t pass:
￼
It’s failing on the last exception. By the time we run the last exception the AuthService.isAuthenticated() function hasn’t yet resolved to a value. Therefore the needsLogin property on the LoginComponent hasn’t been updated.

There are a few ways we can handle asynchronous code in our tests, one is the jasmine way and two are angular specific, let start with Jasmine way.

Jasmines done function
Jasmine has a built -in-way to handle async code and that’s by the passed in done function in the test specs.
So far we’ve been defining our test specs without any parameters but it can take a parameter, a done function which we call when all the async processing is complete, like so:
it('Button label via jasmine.done', (done) => {  //1
  fixture.detectChanges();
  expect(el.nativeElement.textContent.trim()).toBe('Login');
  let spy = spyOn(authService, 'isAuthenticated').and.returnValue(Promise.resolve(true));
  component.ngOnInit();
  spy.calls.mostRecent().returnValue.then(() => {  //2
    fixture.detectChanges();
    expect(el.nativeElement.textContent.trim()).toBe('Logout');
    done();  //3
  });
});

1. The jasmine test spec function is passed a function as the first param, we usually call this parameter done.
2. We can add a callback function(using the spy) which is called when the promise returned from isAuthenticated function us resolved. In this function we know that the component has the new values of needsLogin and we can add our additional expectation here.
3. We we are done with our asynchronous tasks we tell jasmine via the done function.
Jasmine let us create asynchronous tests by giving us an explicit done function which we call when the test is complete.
Although it works trying to understand the code can be difficult as it jump about and is not executed in the order it’s written in.

async and whenState
Angular has another method for us to test asynchronous code via the async and whenState functions.
Let’s rewrite the above test to use these and then we will explain the differences.
it('Button label via async() and whenStable()', async(() => {  //1
  fixture.detectChanges();
  expect(el.nativeElement.textContent.trim()).toBe('Login');
  spyOn(authService, 'isAuthenticated').and.returnValue(Promise.resolve(true));
  fixture.whenStable().then(() => {  //2
    fixture.detectChanges();
    expect(el.nativeElement.textContent.trim()).toBe('Logout');
  });
  component.ngOnInit();
}));

1. We wrap out test spec function in another function called async.
2. We place the tests we need to ru after the isAuthenticated promise resolves inside this function.
This async function executes the code inside it’s body in a special async test zone. This intercepts and keep track of all promises created in its body.

Only when all of those pending promise have been resolved does it then resolves the promise returned from whenStable.

So by using the async and whenStable functions we now don’t need to use  the Jasmine spy mechanism of detecting when the isAuthenticated promise has been resolved, like the previous example.

This mechanism is slightly better than using the plain Jasmine solution but there is another version which gives us find grained control and also allow us to lay out our test code as if it we synchronous.

fakeAsync and tick
it('Button label via fakeAsync() and tick()', fakeAsync(() => {  //1
  expect(el.nativeElement.textContent.trim()).toBe('');
  fixture.detectChanges();
  expect(el.nativeElement.textContent.trim()).toBe('Login');
  spyOn(authService, 'isAuthenticated').and.returnValue(Promise.resolve(true));
  component.ngOnInit();

  tick();  //2
  fixture.detectChanges();
  expect(el.nativeElement.textContent.trim()).toBe('Logout');
}));

1. Like async we wrap the test spec function in a function called fakeAsync.
2. We call tick() when there are pending asynchronous activities we want to complete
Like the async function the fakeAsync function executes the code inside it’s body in a special fake async test zone. This intercepts and keep track of all promise created in it’s body.
The tick() function block execution and simulates the passage of time until all pending asynchronous activities complete.
So when we call tick() the application sits and wait for the promise returned from isAuthenticated to be resolved and then lets execution move to the next line.
The code above is now layer our linearly, as if we were executing synchronous code, there are no callbacks to confuse the mind and everything is simpler to understand.
Important: fakeAsync does have some drawbacks, it doesn’t track XHR requests for instance.

Testing Dependency Injection

Resolving via TestBed
The TestBed acts as a dummy Angular Module and we can configure  it like one include with a set of providers like so:
TestBed.configureTestingModule({
  providers: [AuthService]
});
We can then ask the TestBed to resolve a token into a dependency using its internal injector, like so: 
testBedService = TestBed.get(AuthService);
If most of our test specs need the same dependency mocked the same way we can resolve it once in the beforeEach function and mock it there.

Resolving via the inject function
it('Service injected via inject(...) and TestBed.get(...) should be the same instance',
    inject([AuthService], (injectService: AuthService) => {
      expect(injectService).toBe(testBedService);
    })
);
The inject function wraps the test spec function but lets us also inject dependencies using the parent injector in the TestBed.

We use it like so:
inject(
  [token1, token2, token2],
  (dep1, dep2, dep3) => { }
)
The first param is an array of tokens we want to resolve dependencies for, the second parameter a function whose arguments are the resolved dependencies.
Using inject function:
* Makes it clear what dependencies each spec function uses.
* If each test specs required different mocks and spys this is a better solution that resolving it once per test suite.
Important: This eventually move to becoming an function decorator like so:
@Inject(dep1: Token1, dep2: Token2) => {…}

Overriding the components providers
Before we create a component via the TestBed we can override it’s providers. Lets imagine we have a mock AuthService like so:
class MockAuthService extends AuthService {
  isAuthenticated() {
    return 'Mocked';
  }
}
We can override the components providers to use this mocked AuthService like so.
TestBed.overrideComponent(
    LoginComponent,
    {set: {providers: [{provide: AuthService, useClass: MockAuthService}]}}
);
This syntax is pretty specific, it’s called a MetaDataOverride and it can have the properties set, add and remove. We use set to completely replace the providers array with the values.

Resolving via the component injector
Now our component has been configured with it’s own providers it will therefore have a child injector.
When the component is created since it has it’s own injector it will resolve the AuthService itself and not forward the request to it’s parent TestBed injector.
If we wanted to get the same instance of dependency injection that was passed to the component constructor we need to resolve using the component injector, we can do that through the component fixture like so:
componentService = fixture.debugElement.injector.get(AuthService);
The above code resolves the token using the components child injector.

Testing Components

Test setup
We’ll continue with our  example of testing a LoginComponent. We are going to change our component into a more complex version with inputs, outputs, a domain model and a form, like so:
import {Component, EventEmitter, Input, Output} from '@angular/core';

export class User {  //1
  constructor(public email: string, public password: string) {
  }
}

@Component({
  selector: 'app-login',
  template: `
<form>
  <label>Email</label>
  <input type="email"
         #email>
  <label>Password</label>
  <input type="password"
         #password>
  <button type="button"  //2
          (click)="login(email.value, password.value)"
          [disabled]="!enabled">Login
  </button>
</form>
`
})
export class LoginComponent {
  @Output() loggedIn = new EventEmitter<User>();  //3
  @Input() enabled = true;  //4

  login(email, password) {  //5
    console.log(`Login ${email} ${password}`);
    if (email && password) {
      console.log(`Emitting`);
      this.loggedIn.emit(new User(email, password));
    }
  }
}

1. We create a User class which holds the model of a logged in user.
2. The button is sometimes disabled depending on the enabled input property value and on clicking the button we call the login function.
3. The component has an output event called loggedIn.
4. The component has an input property called enabled.
5. In the login function we emit a new user model on the loggedIn event.
The component is more complex and uses inputs, outputs and emits a domain model on the output event.
Note: We are not using AuthService any more
We also bootstrap our test suite file like so:
describe('Component: Login', () => {

  let component: LoginComponent;
  let fixture: ComponentFixture<LoginComponent>;
  let submitEl: DebugElement;
  let loginEl: DebugElement;
  let passwordEl: DebugElement;

  beforeEach(() => {

    TestBed.configureTestingModule({
      declarations: [LoginComponent]
    });

    // create component and test fixture
    fixture = TestBed.createComponent(LoginComponent);

    // get test component from the fixture
    component = fixture.componentInstance;

    submitEl = fixture.debugElement.query(By.css('button'));
    loginEl = fixture.debugElement.query(By.css('input[type=email]'));
    passwordEl = fixture.debugElement.query(By.css('input[type=password]'));
  });
});
We just have a few more debug element stored on our test suite which we’ll inspect or interact with in our test specs.

Testing @Inputs
To test inputs we need to do things
1. We need to be able to change the input property enabled on our component.
2. We need to check that the button is enabled or disabling on the value of our input property.
Solving the first is actually very easy.
Just because it’s an @Input doesn’t change the fact it’s a still just a simple property which we can change like any other property, like so:
it('Setting enabled to false disables the submit button', () => {
  component.enabled = false;
});
For the second we need to check the disable property value of the buttons DOM element like so:
it('Setting enabled to false disables the submit button', () => {
    component.enabled = false;
    fixture.detectChanges();
    expect(submitEl.nativeElement.disabled).toBeTruthy();
});
Note: We also need to call fixture.detectChanges() to trigger change detection and update the view.

Testing @Output
Testing outputs its somewhat tricker, especially if we want to test from the view.
Firstly let see how we can track what get emitted by the output event and some exception for it.
it('Entering email and password emits loggedIn event', () => {
  let user: User;

  component.loggedIn.subscribe((value) => user = value);

  expect(user.email).toBe("test@example.com");
  expect(user.password).toBe("123456");
});

The key line above is 
component.loggedIn.subscribe((value) => user = value);

Since the output event is actually an Observable we can subscribe to it and get a callback for every item emitted.
We store the emitted value to a user object and then add some expectations on the user object.
How do we actually trigger an event to be fired? We could call the component.login(…) function ourselves but for the purposes of this lecture we want to trigger the function from the view.
Firstly lets set some values to our email and password input controls in the view. We’ve already got references to both those fields in our setup function so we just set the values like so:
loginEl.nativeElement.value = "test@example.com";
passwordEl.nativeElement.value = "123456";
Next we trigger a click on the submit button, but we was to do that after we’ve subscribed to our observable like so:
it('Entering email and password emits loggedIn event', () => {
  let user: User;
  loginEl.nativeElement.value = "test@example.com";  //1
  passwordEl.nativeElement.value = "123456";

  component.loggedIn.subscribe((value) => user = value);

  submitEl.triggerEventHandler('click', null);   //2

  expect(user.email).toBe("test@example.com");
  expect(user.password).toBe("123456");
});

1. Setup data in our input controls
2. Trigger a click on our submit button, this synchronously emits the user object in the subscribe callback!

Testing Directive
We are going to test a directive called the HoverFocusDirective. It Has an attribute selector of hoverfocus and if its attached to an element hovering over that element sets the background color to blue.
import {
    Directive,
    HostListener,
    HostBinding
} from '@angular/core';

@Directive({
  selector: '[hoverfocus]'
})
export class HoverFocusDirective {

  @HostBinding("style.background-color") backgroundColor: string;

  @HostListener('mouseover') onHover() {
    this.backgroundColor = 'blue';
  }

  @HostListener('mouseout') onLeave() {
    this.backgroundColor = 'inherit';
  }
}
It uses @HostListener to listen to mouseover and mouse out events on its host element and it also uses @HostBinding to set the style property of its host element.
The starting point for our test suite is very similar to  our previous example:
import {TestBed} from '@angular/core/testing';
import {HoverFocusDirective} from './hoverfocus.directive';

describe('Directive: HoverFocus', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [HoverFocusDirective]
    });
  });
});

Test wrapper component
To test a directive we typically create a dummy testing components so we can interact with the directive and test it’s effect on the components view, like so:
@Component({
  template: `<input type="text" hoverfocus>`  //1
})
class TestHoverFocusComponent {
}

1. The directive is associated with an input control in the components view.
Now we have component to work with we can configure the test bed and get the required references for the test, like so:
describe('Directive: HoverFocus', () => {

  let component: TestHoverFocusComponent;
  let fixture: ComponentFixture<TestHoverFocusComponent>;
  let inputEl: DebugElement;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [TestHoverFocusComponent, HoverFocusDirective]  //1
    });
    fixture = TestBed.createComponent(TestHoverFocusComponent);  //2
    component = fixture.componentInstance;
    inputEl = fixture.debugElement.query(By.css('input'));
  });
});

1. We declare both the directive we want to test and dummy test component
2. We grab a reference to the component fixture as well as the component and the input DebugElement from the component view.

Interacting and inspecting the view
We now have all the pieces we need to create a test spec for the directive itself:
it('hovering over input', () => {
  inputEl.triggerEventHandler('mouseover', null);  //1
  fixture.detectChanges();
  expect(inputEl.nativeElement.style.backgroundColor).toBe('blue');  //2

  inputEl.triggerEventHandler('mouseout', null);
  fixture.detectChanges();
  console.log(inputEl.nativeElement.style.backgroundColor);
  expect(inputEl.nativeElement.style.backgroundColor).toBe('inherit');
});

1. We use triggerEventHandler to simulate events
2. The style property on the nativeElement is what we can inspect to see the current style applied to an element.

Testing Model Driven Forms
To test forms we’ll extend the LoginComponent. However we’ll convert the simple email, password field form into a model driven form and we’ll drip the input enabled property.
@Component({
  selector: 'app-login',
  template: `
<form (ngSubmit)="login()"  //1
      [formGroup]="form">  //2
  <label>Email</label>
  <input type="email"
         formControlName="email">  //3
  <label>Password</label>
  <input type="password"
         formControlName="password">  //3
  <button type="submit">Login</button>
</form>
`
})
export class LoginComponent {
  @Output() loggedIn = new EventEmitter<User>();
  form: FormGroup;

  constructor(private fb: FormBuilder) {
  }

  ngOnInit() {  //4
    this.form = this.fb.group({
      email: ['', [
        Validators.required,
        Validators.pattern("[^ @]*@[^ @]*")]],
      password: ['', [
        Validators.required,
        Validators.minLength(8)]],
    });
  }

  login() {
    console.log(`Login ${this.form.value}`);
    if (this.form.valid) {
      this.loggedIn.emit(
          new User(
              this.form.value.email,
              this.form.value.password
          )
      );
    }
  }
}

1. When the user submits the form we call the login() function.
2. We associate this template form element with the model form on our component
3. We link specific template form controls to FormControls on our form model.
4. We initialize our form  model in the ngOnInit lifecycle hook.
The rest of the LoginComponent looks the same as before.
Our test suite look mostly the same but has a few key differences:
describe('Component: Login', () => {

  let component: LoginComponent;
  let fixture: ComponentFixture<LoginComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [ReactiveFormsModule, FormsModule],  //1
      declarations: [LoginComponent]
    });

    // create component and test fixture
    fixture = TestBed.createComponent(LoginComponent);

    // get test component from the fixture
    component = fixture.componentInstance;
    component.ngOnInit();  //2
  });
});
1. We add the required ReactiveFormsModule and FormsModule to our test beds import list.
2. We manually trigger the ngOnInit lifecycle function on our component, Angular won’t call this for us.

Form validity
The first spec we may want to check is that a blank form is invalid. Since we are using model driven forms we can just check the valid property on the form model itself, like so:
it('form invalid when empty', () => {
  expect(component.form.valid).toBeFalsy();
});
We can easily check to see if the form is valid by checking the value of component.form.valid.
Tip:This one of the reason model driven form are easier to test than template driven forms, we already have an object on the component we can inspect form our test spec for correctness.
With template driven forms the state is in the view and unless the components has a reference to the template form with a ViewChild decorator there is no way to test the form using a unit test. We would have to perform a full E2E test simulating button clicks and typing in values into forms.

Field validity
We can also check to see if individual fields are valid, for example the email field should initially be invalid.
it('email field validity', () => {
  let email = component.form.controls['email'];  //1
  expect(email.valid).toBeFalsy();  //2
});
1. We grab a reference to the actual field itself from the form.controls property.
2. Just like the form we can check if the field is valid through email.valid.

Field errors
As well as checking to see if the field is valid we can also see what specific validators are failing through email.errors property.
Since it’s required and the email field hasn’t been set I would expect the required validator to be failing we can test for this like so:
  it('email field validity', () => {
    let errors = {};
    let email = component.form.controls['email'];
    errors = email.errors || {};
    expect(errors['required']).toBeTruthy();  //1
  });
1. Because errors contains a key of required and this has a value this means the required validator is failing as we expect.
We can set some data on our input control by calling setValue(…) like so:
email.setValue("test");
If we did set the email field to be test this should fail the pattern validator, since that expects the email to contain a @. We can then check to see if the pattern validator is failing like so:
email.setValue("test");
errors = email.errors || {};
expect(errors['pattern']).toBeTruthy();

Submitting a form
We can submit a form by clicking on the submit button. Since the ngSubmit directive has it’s own set of tests it’s safe to assume that the (ngSubmit)= login() expression is working as expected.
So to test form submission with model driven forms we can just call the login() function on our controller, like so:
component.login();
Since our form emits an event form the loggedIn output event property we can use the same method we covered in the section on testing component to test this form submission. Namely subscribe to the observable and store a reference to the emitted event for later comparison, like so:
  it('submitting a form emits a user', () => {
    expect(component.form.valid).toBeFalsy();
    component.form.controls['email'].setValue("test@test.com");
    component.form.controls['password'].setValue("123456789");
    expect(component.form.valid).toBeTruthy();

    let user: User;
    // Subscribe to the Observable and store the user in a local variable.
    component.loggedIn.subscribe((value) => user = value);

    // Trigger the login function
    component.login();

    // Now we can check to make sure the emitted value is correct
    expect(user.email).toBe("test@test.com");
    expect(user.password).toBe("123456789");
  });

Testing Http
To demonstrate how to test http request we will add a test for our iTunes SearchService.
We will use the promise version of the search service that uses JSONP to get around the issue of CORS.
import {Injectable} from '@angular/core';
import {Jsonp} from '@angular/http';
import 'rxjs/add/operator/toPromise';

class SearchItem {
  constructor(public name: string,
              public artist: string,
              public thumbnail: string,
              public artistId: string) {
  }
}

@Injectable()
export class SearchService {
  apiRoot: string = 'https://itunes.apple.com/search';
  results: SearchItem[];

  constructor(private jsonp: Jsonp) {
    this.results = [];
  }

  search(term: string) {
    return new Promise((resolve, reject) => {
      this.results = [];
      let apiURL = `${this.apiRoot}?term=${term}&media=music&limit=20&callback=JSONP_CALLBACK`;
      this.jsonp.request(apiURL)
          .toPromise()
          .then(
              res => { // Success
                this.results = res.json().results.map(item => {
                  console.log(item);
                  return new SearchItem(
                      item.trackName,
                      item.artistName,
                      item.artworkUrl60,
                      item.artistId
                  );
                });
                resolve(this.results);
              },
              msg => { // Error
                reject(msg);
              }
          );
    });
  }
}

Note: Although we are using JSONP here , testing Http and Jsonp is exactly the same. We just replace instance of Jsonp with Http.

Configuring the test suite
We want the Jsonp and Http services to use the MockBackend instead of the real Backend, this underling code that actually sends and handle http.
By using the MockBackend we can intercept real requests and simulate responses with test data.
The configuration is slightly more complex since we are using a factory provider.
{
  provide: Http,  //1
  useFactory: (backend, options) => new Http(backend, options),  //2
  deps: [MockBackend, BaseRequestOptions]  //3
}
1. We are configuring a dependency for the token Http.
2. The injector calls this function in order to return a new instance of the Http class. The arguments to the useFactory function are themselves injected in.
3. We define the dependencies to our useFactory function via the reps property.
For our API however we are using JSOPN, we can just replace all mention of Http with Jsonp like so:
{
  provide: Jsonp,
  useFactory: (backend, options) => new Jsonp(backend, options),
  deps: [MockBackend, BaseRequestOptions]
}
The above configuration ensures that the Jsonp service is constructed using the MockBackend so we can control it later on in testing.
Together with the other providers and modules we need our initial test suite file look like so:
describe('Service: Search', () => {

  let service: SearchService;
  let backend: MockBackend;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [JsonpModule],
      providers: [
        SearchService,
        MockBackend,
        BaseRequestOptions,
        {
          provide: Jsonp,
          useFactory: (backend, options) => new Jsonp(backend, options),
          deps: [MockBackend, BaseRequestOptions]
        }
      ]
    });

    backend = TestBed.get(MockBackend);  //1

    service = TestBed.get(SearchService);  //2
  });
});
1. We grab a reference to the mock backend so we can control the http response form our test specs.
2. We grab a reference to the SearchService, this has been created using the MockBackend above.

Using the MockBackend to simulate a response
Just by using the MockBackend instead of the real Backend we have stopped the tests from triggering real http requests from being sent out.
Now we need to configure the MockBackend to return dummy test data instead, like so:
it('search should return SearchItems', fakeAsync(() => {
  let response = {  //1
    "resultCount": 1,
    "results": [
      {
        "artistId": 78500,
        "artistName": "U2",
        "trackName": "Beautiful Day",
        "artworkUrl60": "image.jpg",
      }]
  };

  backend.connections.subscribe(connection => {  //2
    connection.mockRespond(new Response(<ResponseOptions>{  //3
      body: JSON.stringify(response)
    }));
  });
}));
1. We create some fake data we want the API to response with.
2. The mock backend connections property is an observable that emits a connection every time an API request is made.
3. For every connection that is request we tell it to  mockRespond with our dummy data.
The above code returns the same dummy data for every API request, regardless of the URL.

Testing the response
Using HTTP is asynchronous so in order to test we need to use one of the asynchronous testing methods, we’ll use the fakeAsync methodit('search should return SearchItems', fakeAsync(() => {  //1
  let response = {
    "resultCount": 1,
    "results": [
      {
        "artistId": 78500,
        "artistName": "U2",
        "trackName": "Beautiful Day",
        "artworkUrl60": "image.jpg",
      }]
  };

  // When the request subscribes for results on a connection, return a fake response
  backend.connections.subscribe(connection => {
    connection.mockRespond(new Response(<ResponseOptions>{
      body: JSON.stringify(response)
    }));
  });

  // Perform a request and make sure we get the response we expect
  service.search("U2");  //2
  tick();  //3

  expect(service.results.length).toBe(1);  //4
  expect(service.results[0].artist).toBe("U2");
  expect(service.results[0].name).toBe("Beautiful Day");
  expect(service.results[0].thumbnail).toBe("image.jpg");
  expect(service.results[0].artistId).toBe(78500);
}));
1. We use the fakeAsync method to execute in the special fake async zone and track pending promises.
2. We make the asynchronous call to service.search(…)
3. We issue a tick() which blocks execution and waits for all the pending promises to be resolved.
4. We now know that the services has received and parsed the response so we can write some expectations.

Testing Routing
To test routing we need a few components and a route configuration.
import {Component} from "@angular/core";
import {Routes} from "@angular/router";

@Component({
  template: `Search`
})
export class SearchComponent {
}

@Component({
  template: `Home`
})
export class HomeComponent {
}

@Component({
  template: `<router-outlet></router-outlet>`
})
export class AppComponent {
}

export const routes: Routes = [
  {path: '', redirectTo: 'home', pathMatch: 'full'},
  {path: 'home', component: HomeComponent},
  {path: 'search', component: SearchComponent}
];

We create three components HomeComponent, SearchComponent and an AppComponent with a <router-outlet>.

We also create a route configuration where ‘’ redirects you to home and home search show their respective components.
Our basic test suite look like so:
import {Location} from "@angular/common";
import {TestBed, fakeAsync, tick} from '@angular/core/testing';
import {RouterTestingModule} from "@angular/router/testing";
import {Router} from "@angular/router";

import {
    HomeComponent,
    SearchComponent,
    AppComponent,
    routes
} from "./router"

describe('Router: App', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [  //1
        HomeComponent,
        SearchComponent,
        AppComponent
      ]
    });
  });
});

1. We import and declare our components in the test bed configuration.

Router setup
Normally to setup routing in an Angular application we import the RouterModule and provide the routes to the NgModule with RouterModule.withRoutes(routes).
However when testing routing we use the RouterTestingModule instead. This modules setup the router with a spy implementation of the Location Strategy that doesn’t actually change the URL.
We also need to get the injected Router and Location so we can use them in the test specs.
Our test suite file now looks like:
describe('Router: App', () => {

  let location: Location;
  let router: Router;
  let fixture;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [RouterTestingModule.withRoutes(routes)],  //1
      declarations: [
        HomeComponent,
        SearchComponent,
        AppComponent
      ]
    });

    router = TestBed.get(Router);  //2
    location = TestBed.get(Location);  //3

    fixture = TestBed.createComponent(AppComponent);  //4
    router.initialNavigation();  //5
  });
});
1. We import our RouterTestingModule with our routes
2. We grab a reference to the injected Router
3. We grab reference to the injected Location.
4. We ask the test best to create an instance of our root AppComponent. We don’t need this reference in out test specs but we do need to create the root component with the router-outlet so the router has somewhere to insert components.
5. This sets up the location change listener and performs the initial navigation.

## Testing routing
In our configuration we’ve set it so if you land on the root empty url you will be redirected to /home, lets add a test spec for this:
```javascript
it('navigate to "" redirects you to /home', fakeAsync(() => {  //1
  router.navigate(['']);  //2
  tick(); //3
  expect(location.path()).toBe('/home');  //4
}));
```
1. Routing is an asynchronous activity so we use one of the asynchronous testing methods at our disposal, in this case the fakeAsync method.
2. We trigger the router to navigate to the empty path.
3. We wait for all pending promises to be resolved.
4. We can then inspect the path our application should be at with location.path().
Let also add a test spec for navigating to the search route, like so:
```javascript
it('navigate to "search" takes you to /search', fakeAsync(() => {
  router.navigate(['search']);
  tick();
  expect(location.path()).toBe('/search');
}));
```
The spec is exactly the same as the previous one, our links params array is different since we are triggering a different route and out expectation is again different, but the rest is the same.

# Isolated vs Shallow vs Integrated
When we want to test Angular Component, we have three different possible way to do it: isolated testing, shallow testing or integrated testing.
Here is the annotate code for the ones that can’t wait, we  will see the difference right after. You can play with it in the Stackbiltz below, in the integrated-isolated-shalow folder.
```javascript
import { ComponentFixture, TestBed, async } from '@angular/core/testing';
import { By } from '@angular/platform-browser';
import { NO_ERRORS_SCHEMA } from '@angular/core';
import { RouterTestingModule } from '@angular/router/testing';
import { Router } from '@angular/router';
// app
import { CounterComponent } from './counter.component';
import { MenuComponent } from './menu.component';

/**
 * This component has some logic in it, and a very simple template.
 * For now, we only want to test the class logic. For doing so, 
 * we will test only the component class without rendering the template.
 * This test is an Isolated test.
 */
describe('CounterComponent (isolated test)', () => {
  it('should instantiate', () => {
    const component: CounterComponent = new CounterComponent();
    expect(component).toBeDefined();
  });

  it('should start with a counter at `0`', () => {
    const component: CounterComponent = new CounterComponent();
    expect(component.counter).toEqual(0);
  });

  it('should be able to increment the counter (+1)', () => {
    const component: CounterComponent = new CounterComponent();
    component.counter = 5;

    component.increment();
    component.increment();

    expect(component.counter).toEqual(7);
  });

  it('should be able to decrement the counter (-1)', () => {
    const component: CounterComponent = new CounterComponent();
    component.counter = 5;

    component.decrement();
    component.decrement();

    expect(component.counter).toEqual(3);
  });
});

/**
 * Now that the inner class' logic is tested.
 * To test if the buttons trigger the right logic, we need to test them.
 * We need to render the template and trigger some clicks.
 * Because this component'template contains another component,
 * and we only want to test the relevant part of the template, we will not
 * render the child component.
 * This is a Shallow test.
 */
describe('CounterComponent (shallow test)', () => {
  let component: CounterComponent;
  let fixture: ComponentFixture<CounterComponent>;
  
  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [CounterComponent],
      schemas: [NO_ERRORS_SCHEMA]
    }).compileComponents(); // This is not needed if you are in the CLI context
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(CounterComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });

  it('should instantiate', () => {
    expect(component).toBeDefined();
  });

  it('should increment the counter if increment button is clicked (+1)', () => {
    const button = fixture.debugElement.nativeElement.querySelector('.button-up');

    button.click();
    button.click();

    expect(component.counter).toEqual(2);
  });

  it('should decrement the counter if decrement button is clicked (-1)', () => {
    component.counter = 5; // Fake some increment clicks before.
    const button = fixture.debugElement.nativeElement.querySelector('.button-down');

    button.click();
    button.click();

    expect(component.counter).toEqual(3);
  });

  it('should reset the counter if reset button is clicked (0)', () => {
    component.counter = 3; // Fake some increment clicks before.
    const button = fixture.debugElement.nativeElement.querySelector('.button-0');

    button.click();

    expect(component.counter).toEqual(0);
  });
});

/**
 * We could now go deeper and test the whole component with its dependencies,
 * see if everything is working great.
 * This is an Integrated test.
 */
describe('CounterComponent (integrated test)', () => {
  let component: CounterComponent;
  let fixture: ComponentFixture<CounterComponent>;
  let router: Router;
  
  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [CounterComponent, MenuComponent],
      imports: [RouterTestingModule]
    }).compileComponents(); // This is not needed if you are in the CLI context
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(CounterComponent);
    component = fixture.componentInstance;

    router = TestBed.get(Router);
    spyOn(router, 'navigateByUrl');

    fixture.detectChanges();
  });

  it('should instantiate', () => {
    expect(component).toBeDefined();
  });

  it('should trigger the navigation to `/home`', async(() => {
    const link = fixture.debugElement.nativeElement.querySelector('.home-link');

    link.click();

    expect(router.navigateByUrl).toHaveBeenCalled();
  }));

  it('should trigger the navigation to `/about`', async(() => {
    const link = fixture.debugElement.nativeElement.querySelector('.about-link');

    link.click();

    expect(router.navigateByUrl).toHaveBeenCalled();
  }));
});
```
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'app-counter',
  template: `
    <header>
      <app-menu></app-menu>
    </header>
    <main>
      <div>
        Counter is at {{counter}}. 
        <a [routerLink]="['/home']" class="home-link">Go home</a>
      </div>
      <div>
        <button (click)="increment()" class="button-up">increment</button>
        <button (click)="decrement()" class="button-down">decrement</button>
        <button (click)="reset()" class="button-0">reset</button>
      </div>
    </main>
  `
})
export class CounterComponent {
  public counter: number = 0;

  constructor() { }

  public increment() {
    this.counter++;
    return this.counter;
  }

  public decrement() {
    this.counter--;
    return this.counter;
  }

  public reset() {
    this.counter = 0;
    return this.counter;
  }

}
```
## Isolated Testing
When we use the isolated test on a component, we usually want to test its (complex) logic above all, we don’t want to focus on its template.

We can take the CounterComponent. This component has some logic we want to test and a very simple template. Its template has some custom DOM elements which are in this case, other Angular components and directives.
Because we want to focus on the component’s logic the template is irrelevant. We will do, in that case, an isolated test, focusing only on the component class.
An isolated test only focus on the component’s class.

## Shallow testing
When we use the shallow test on a component, we usually want to test its own template with the class logic. By doing so, we will mock all the other components that are part of the template.
We can see that the CounterComponent template has some buttons that we can click on. Furthermore, we can see that the template has still some custom DOM elements and directives. If we test it right away, Angular will throw an error to tell us that it doesn’t know some template syntaxes. These custom elements are not what we want to test right now. We only want to focus our test on the direct component’s template.
To be able to only focus on the component’s template and not its dependencies, we need to tell to not try to compile those custom elements. This is exactly what schemas: [NO_ERROR_SCHEME] do by telling to Angular to not throw any error when it doesn’t recognize a custom elements, in the TestBed configuration.
The shadow test focuses on the component’s class and its template, with out its dependencies by mocking them.

## Integrated test
When we use an integrated test on a component, we want to test its class logic, it template and all its dependencies. Meaning, if its template includes custom elements (Angular components or directives), we will need to satisfy all those dependencies by injecting them in the TestBed configuration.
This test is the most heavy as it tests the component as a whole. Even if it looks like the shallow test, it tests more design and goes deeper. It could be more costly to main and harder to debug too, because you need to handle its dependencies.
An integrated test will test the component and its dependencies as a whole.


