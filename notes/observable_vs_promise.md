# Javscript Promise vs RxJS Observables

## Asynchronous Programming in Javascript
 First of all, let's recall what promises and observables are all about: handling asynchronous execution. There are different ways in Javascript to create asynchronous code. The most important ones are following:
 - Callbacks
 - Promises
 - Async/Await
 - RxJS Observables

 ### Callbacks
 This is the old-fashioned classical approach to asynchronous programming. You provide a function as an argument to another function that executes an asynchronous task. When the asynchronous task completes, the executing function calls your callback function.
 The main disadvantage of this approach occurs when you have multiple chained asynchronous tasks, which required you to define callback functions within callback functions within callback function... This is called **callback hell**.

 ### Promises
 Promises have been introduced in ES6(2015) to allow for more readable asynchronous code than is possible with callbacks.

 The main difference between callbacks and promise is that with callbacks you **tell executing function** what to do when the asynchronous task completes, whereas with promises the executing function returns a special object to you(the promise) and then you **tell the promise** what to do when the asynchronous task completes.

 In practive, this look like this:
 ```javascript
 const promise = asyncFunc();
 promise.then(result => console.log(result));
 ```
 That is, instead of providing a function reference as an argument to `asyncFunc` (as you would with callbacks), `asyncFunc` immediately return a promise to you and then you provide the action to be taken when the asynchronous task completes to this promise (through its `then` method).

 ### Async/Await
 Async/await has been introduced in ES8(2017). This technique should really be listed under *promises*, because it is syntactic sugar for working with promises. However, it is a syntatic sugar that is really worth looking at.

 Basically, you can declare a function to be `async`, which allows you to use the `await` keyword in the body of this function. The `await` keyword can be put in front of an expression of the `async` function until the promise is resolved. When this happens, the entire `await` expression evaluates to the result value of the promise, and then the execution of the `async` function resumes.

 Furthermore, the `async` function itself returns a promise as well that is resolved when the execution of the function body completes.

 Let's see how this look like in practive with the following example: 
 ```javascript
 function asyncTask(i) {
     return new Promise(resolve => resolve(i +1));
 }

 async function runAsyncTasks() {
     const res1 = await asyncTask(0);
     const res2 = await asyncTask(res1);
     const res3 = await asyncTask(res2);
     return "Everything done";
 }

 runAsyncTasks().then( result => console.log(result));
 ```
 The `asyncTask` function implements an asynchronous task that takes an argument and return a result. The function returns a promise that is resolved when the asynchronous task completes. There is nothing special about this function, it's just a normal function that a returns promise.

 The `runAsyncTasks` function, on the other hand, is declared `async` so that the `await` keyword can be used in its body. This function calls `asyncTask` three times, and each time the argument must be the result of the preceding call to `asyncTask`(i.e. we are chaining three asynchronous tasks).

The first `await` keyword causes the execution of `runAsyncTasks` to stop until the promise returned by `asyncTask(0)` is resolved. The entire `await asyncTask(0)` expression then evaluates to the result value of the resolved promise and is assigned to `res1`. At this point, `asyncTask(res1)` is called and the second `await` keyword cause execution of `runAsyncTasks` to stop again until the promise returned by `asyncTask(res1)` is resolved. This continues until eventually all the statements in the `runAsyncTasks` function body have been executed.

As mentioned, an `async` function returns a promise itself that is resolved with the function's return value when the execution of the function body completes. So, in other words, an `async` function is itself an asynchronous task( that typically manages the execution of other asynchronous tasks). This can be seen in the last line where we call the `then` function on the returned promise to print out the `async` function's return value.

I claimed that async/await is just syntactic sugar for promise. If this is true, then we must be able to implement the above example with pure promises. Yes, we can, and this is how it would look like:
```javascript
function asyncTask(i) {
    return new Promise(resolve => resolve(i+1));
}

function runAsyncTasks() {
    return asyncTask(0)
    .then( res1 => { return asyncTask(res1);})
    .then( res2 => { return asyncTask(res2);})
    .then( res3 => { return "Everthing done"; });
}
runAsyncTasks().then(result => console.log(result));
```
This code is equivalent to the async/await version, and if you add appropriate log statements in the anonymous function bodies, then it produces the same output as the async/await version.

The one thing that has changes in the `runAsyncTasks` function. It is now a regular function (not `async`) and it uses `then` to chain the promises returned by `asyncTask` (instead of `await`).

I think it is needless to say that the async/await version is much more readable and easier to understand than the promise version. In facr, the main innovation of async/await is to allow to write asynchronous code with promises that "look like" synchronous code.

### RxJS Observables
To begin with, RxJS is the Javascript implementation of the  **ReactiveX** project. The ReactiveX project aims at providing an API for asynchronous programming for different programming languages.

The fundamental paradigm of ReactiveX is the Gang of Four **observer pattern** (ReactiveX even extends the observer pattern with completion and error notification). Therefore the central abstracton of all ReactiveX implementations is the observable. 

The ReactiveX API is implemented in various programming languages. As mentioned, RxJS is the Javascript implementation of ReactiveX. Besides that, there exists, for example, RxJava(Java), RxKotlin (Kotlin), Rx.rb (Ruby), RxPY(Python), RxSwift (Swift), Rx.NET(C#) implementations, and many more.

So, now we know what RxJS is, but **what is an observable?** Let's try to characterise it along two dimensions and compare it with other known abstractions. The dimensions are **synchronicity/asynchronicity** and **single value/ multiple values**.
- Emits multiples values
- Emits its values asynchronously ("push")

Let's constrast this with **promises**, that we just introduced in the previous subsection:
- Emits a single value
- Emits its value synchronously ("pull")

Note that with *synchronous/pull* and *asynchronous/push* I mean the following: **synchronous/pull** means that the client code *requests* a value from the abstraction and *blocks* until this value is returned. **Asynchronous/push** means that the abstraction notifies the client code that a new value is being emitted and the client code *handles* this notification.

If we arrange these abstractions along the dimensions graphically, we get the following picture(taken from ReactiveX)
||Single Value| Multiple Values|
|Synchronous| Get | Iterable|
|Asynchronous| Promise | Observable|

Note that we didn't yet mention **Get**, but this stands just for a normal data access operation such as regular function call.

Looking at above picture, we could say that an *observable* is to an interable what a *promise* is to a *get* operation Or that a *promise* is like an *asynchronous* get operation where an *observable* is like an *asynchronous iterable*.

We could also say that the main difference between a promise and an observable is that a promise emits only a single value, whereas an observable emits multiple values.

But let's look at it in more detail. With a simple get operation, for example a function call, the calling code requests a single value and then waits or blocks until the function returns this value (the calling code *pulls* the value).

With a **promise**, on the other hand, the calling code also request a single value, but it doesn't block until the value is returned. It just kicks off the computation and then goes on with the execution of its own code. When the promise finishes the computation of the value, it emits the value to the calling code, which then handles the value ( the value is *pushed* to the calling code).

Now, let's look at iterable. In may programming languages we can create an iterable from a collection data structure, such as an array. They iterable typically has a `next` method, which returns the next unread value from the collection. The calling code can then repeatedlly call `next` in order to read all the values of the collection. Each `next` call is basically a synchronous blocking `get` operation as explained above (the calling code repeatedly pulls values).

An **observable** take the iterable to the asynchronous world. Like an iterable, an observable computes and emits a stream of values. However, unlike an iterable, with an observable the calling code does not synchronously 'pull' each value, but the observable asynchronously pushes each value to the calling code, as soon as it is avialable. To this end, the calling code provides a handler function to the observable, which in RxJS is called `next`, and the observable then calls this function for each value that it computes.

The values that an observable emits can be anything: the elements of an array, the result of an HTTP request (it's OK if an observable emits just a single value, it doesn't always have to be multiple values), user input events, such as mouse observable can also emit only a single value, an observable can do everthing that a promise can do, but the reverse is not true.

In addition to this, ReactiveX observables provide a large number of so-called **operator**. These are functions that can be applied to an obsevable in order to modify the set of emitted values. Common categories of operator are *combinations, filters and transformations.*

For example, there is a `map` operator that we could configure as follows: `map(value => 2* value)`, and then we can apply this operator to an observable. The effect is that each value that the observable emits is multiplied by two before it is pushed to the calling code.

The following is a short code example showing the creation and usage of an RxJS observable
```javascript
//Creation
const observable = new Observable( observer => {
    for (let i = 0; i < 3; i++) {
        observer.next(i);
    }
});

//Usage
observable.subscribe(value => console.log(value));
```

This concludes our overview of asynchronously programming techniques in Javascript. We have seen that there **callbacks**, which are old-fashioned, **promises**, which can be used to obtain a single value asynchronously, **async/await**, which is syntactic sugar for promises, and *RxJS observables**, which can be used to obtain streams of values asynchronously.

## Promises Vs. Observables
You can install RxJS as follows:
```unix
npm install --save rxjs
```
And you can import the `Observable` constructor (that's all you need for these examples) in your code files as follows:
```javascript
import {Observable} from 'rxjs';
```
However, if you use Node.js, you have to do the import in a different ways as follows (because Node.js does not yet support the `import` statement) :
```javascript
const {Observable} = require('rxjs');
```
### Creation
Let's look at how to create a promise versus how to create an observable. To keep it simple, we will for first neglect errors and only consider "successful" executions of promises and observables.

Note that there are two sides to both promises and observables: **creation** and **usage**. A promise/observable is an object that first of all needs to be *created* by someone. After it is created, it is typically passed to someone else who *uses* it. Creation defines the behavior of a promise/observable and the values that are emitted, and usage defines the handling of these emitted values.

A typical use case is that promise/observables are created by API functions and returned to the user of the API. The user of the API then uses these promise/observables. So, if you use an API, you typically just use promise/observables, whereas if you're the author of an API, you also have to create promises/observables.

In the following, we will first look at _creation_ of promise/observables, and we will look at their usage in a subsequent subsection.

**Promises:**
```javascript
new Promise(executeFunc);

function executeFunc(resolve) {
    // some code ...
    resolve(value);
}
```
To create a promise, you call the `Promise` constructor passing it a so-called _executor_ function as argumenet. The executor function is called by the system when the promise is created, and it is passed as argument a special `resolve` function (you can name this argument however you want, just remember that the *first* argument to the executor function is the resolve function, and you have to use it as such).

When you call the `resolve` function in the body of the executor function, the promise is transferred to `fulfilled` state and the value that you pass as argument to the `resolve` function is "emitted" (this promise is resolved).

**Observables:**
```javascript
new Observable(subscriberFunc);

function subscriberFunc(observer) {
    // Some code...
    observer.next(value);
}
```
To create an observable, you call the `Observable` constructor passing it a so-called _subscriber function_ as argument. The subscriber function is called by the system whenever a new subscriber subscribes to the observable. The subscriber function gets as argument an _observer_ object. This object has a method `next`, which when called, _emit_ the values that you pass it as argument from the observable.

Note that after calling `next`, the subscriber function keeps running, and it can call `next` many more times. This is an important difference to promises, where after calling `resolve` the executor function is terminated. Promises can emit at most one value, whereas observables can emit any number of values.

### Creation (With Error Handling)
The above examples didn't yet show the full capabilities of promise and observables. Errors may occur during the execution of a promise/observable, and both techniques provide means to indicate such errors to the code that "uses" them.

The following extends the above explanations with error handling capabilities.

**Promise:**
```javascript
new Promise(executorFunc);
function executorFunc(resolve, reject) {
    //some code...
    resolve(value);
    //some code...
    reject(error);
}
```
The executor function that you pass to the `Promise` constructor gets actually a second argument, which is the `reject` function. The `reject` function is used to indicate an error in the promise execution. When you call it, the executor function is aborted and the promise is transferred to the _rejected_ state.

On the usage side, this will cause the onRejected function (that you may pass to the `catch` method) to be executed.

**Observables:**
```javascript
new Observable(subscriberFunc);

function subscriberFunc(observer) {
    //some code
    observer.next(value);
    //some code
    observer.error(error);
}
```
The observer object that is passed as argument to the subscriber function actually has one more method: the `error` method. Calling this method indicates an error to the subscriber of the observable.

Unlike `next`, calling the `error` method also terminates the subscriber function, and thus terminates the observable. This means that `error` can be called at most one time during the lifetime of an observable.

`next` and `error` are still not the entire truth. The observer object that is passed to the subscriber function has one more method: `complete`. Its usage is shown in the following:
```javascript
new Observable(subscriberFunc);
function subscriberFunc(observer) {
    //some code
    observer.next(value);
    //some code
    observer.error(error);
    //if all successful
    observer.complete();
}
```
The `complete` method is supposed to be called when an observable successfully "completes". Completing means that there is no more work to, that is, all values have been emitted. Like the `error` method, the `complete` method terminates the execution of the subscriber function, which means that the `complete` method can be called at most one time during the lifetime of an observable.

Note that calling the `complete` method of an observable execution is recommended, but not mandatory.

### Usage
After having covered the creation of Promises and observables, let's now look at their usage. Using a promise or obseravable means "subscribing" to it, which in turn means registering a handler function with the promise or observable that will invoked for each emitted values (one value for a promise, any number of values for an observable).

The registering of the handler function is done through a special method of the method or observalbe object. These methods are, respectively:
- **Promise:** __then__
- **Observalbe:** __subscribe__

In the following we show the basic usage of these methods for both promises and observables.

Note that in the following code snippets we assume that a promise or observable object already exists. So if you want to run the code, you must prepend it with a promise or observable statement, for example:
```javascript
const promise = new Promise(/*...*/)'
```

```javascript
const obervable = new Obseravable(/*...*/);
```
**Promises:**
```javascript
promise.then(onFulfilled);

function onFullfilled(value) {
    //Do something with value...
}
```
Given a promise object, we call the `then` method of this object and pass it an _onFulfilled_ function as argument. The _onFulFilled_ function takes a single argument. This argument is the result value of the promise, that is, the value that has been passed to the `resolve` function inside the promise.

**Observables:**
Using an observable means subscribing to it, and this is done with `subscribe` method of an observable. There are actually two equivalent ways to use the `subscribe` method. In the following wer are going to present both of them:

_Option 1:_
```javascript
observable.subscribe(nextFunc);

function nextFunc(value) {
    //Do something with value...
}
```
In this case, we call the `subscribe` method of an observable and pass it a next function as argument. The _next_ function takes a single argument. This argument is the currently emitted value whenever the observable emits a value.

In other words, whenever the observable's internal subscribe function calls the `next` method, you _next_ function is being called with the value that is passed to `next` (thus emitting the value  from the observabl to your handler function).

_Option 2:_
```javascript
observable.subscribe({
    next: nextFunc
});

function nextFunc(value) {
    //Do something with value...
}
```
The second option might look a bit strange, but actually it shows better what's going on the under the hoods.

In this case we call the `subscribe` not with a function as argument, but with an object. The object has a single property with a ket called `next` and a function a value. This function is nothing else than our good old next function from above.

Anything else stays the same, we just pass the _next_ function inside an object rather than directly as an argument. But why would we wrap our handler function in an object before passing it to the `subscribe` method ?

The object that can be passed to `subscribe` in this way is in fact an object that implements the `Observer` interface. Maybe you remember that when we created observables in the previous subsections, we used to define a subscriber function, and this subscriber function took a single argument that we called `observer`. In particular, we used code like this:
```javascript
new Observable(subscriberFunc);

function subscriberFunc(observer) {
    //Some code...
    observer.next(value);
}
```
The `observer` argument of the subscriber function corresponds directly to the object that we pass to `subscribe` above (actually, the object passed to `subscribe` is first converted from type `Observer` to `Subscriber` before being passed to the subscriber function, and  `Subscriber` implements the `Observer` interface).

So, with option 2, we already create an object that forms the basis of the actual object that will be passed into the subscriber function of the observable, whereas with option 1, we merely provide the functions that will be used as methods of this object.

Which of these options to use is a matter of taste and coding style. Just note that if you use option 2, the object property key for the _next_ function _must_ mandatorily be called `next`. This is dictated by the `Observer` interface which this object is required to implement.

### Usage (With Error Handling)
As before, we now extend the usage examples to include error handling. Error handling in this case means providing a special handler function that handles potential errors that are indicated by the promise or observable (in addition to the "regualr" handler function that handles the "regular" values that are emitted from the promise or observable).

