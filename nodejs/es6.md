# Introduction
ECMAScript is a standardized version of Javascript with the goal of unifying the language's specification and features. As all major browsers and Javascript-runtimes follow this specification, the term ECMAScript is interchangeable with the term Javascript.

## Differences between the `var` and `let` keywords.
One of the biggest problems with declaring variables with the `var` keyword is that you can overwrite variable declarartions without an error.

```javascript
var camper = "James";
var camper = "David";
console.log(camper); //David
```
When you declare a variable with the `var` keyword, it is declared globally, or locally if declared inside a function.

The `let` keyword behaves similarly, but with some extra features. When you declare a variable with the `let` keyword inside a block statement, or expression, its scope is limited to that block statement, or expression.

Declare a read only variable with the `const` keyword.

## Prevent Object Mutation
Javascript provides a function `Object.freeze` to prevent data mutation.

Once the object is frozen, you can no longer add update, or delete properties from it. Any attempt at changing the object will be rejected without an error.

```javascript
 let obj = {
     name: "Imdb",
     review: "Awesome"
 };
 Object.freeze(obj);
 obj.review = "bad"; //will be ignored. Mutation not allowed.
 obj.newProp = "Test"; //will be ignored. Mutation not allowed.
 ```

## Spread Operator
ES6 introduced the spread operator, which allow us to expand arrays and other expressions in places where multiple parameters or elements are expected.

## Create a Module Script
Javascript started with a small role to play on an otherwise mostly HTML web. Today, it's huge and some websites are built almost entirely with Javascript. In order to make Javascript more modular, clean and maintainable; ES6 introduced a way to easily share code among Javascript files. This involves exporting parts of a file for use in one or more other files, and importing the parts you need, where you need them.

## Use export to share a Code block
Imagine a file called `math_function.js` that contains several functions related to mathematical operations. One of them is stored in a variable, add that takes in two numbers and return their sum. You want to use this function in several different Javascript files. In order to share it with these other files, you first need to export it.

```javascript   
export const add = (x,y) => {
    return x+y;
}
```

`export` syntax you need to know, known as _export default_. Usually you will use this syntax if only one value is being exported from a file. It is also to create a fallback value for a file module.

```javascript
export default function add(x,y) {
    return x+y;
}

export default function(x,y) {
    return x+y;
}
```

`import` allow you to choose which parts of a file or module to load. Here's how you can import it to use in another file.

```javascript
import {add} from './math_function.js';
```
To import a default export you need to use a different `import` syntax. In the following example, `add` is the default export of the `math_function.js` file. Here is how to import it.

```javascript
import add from "./math_functions.js";
```

## Promise
A promise in Javascript is exactly what is sound like - you use it to make a promise to do something, usually asynchronously. When the task completes, you either fulfill your promise or fail to do so. `Promise` constructor function, so you need to use the `new` keyword to create one. It takes a function, as it argument, with two parameters - `resolve` and `reject`. These are methods used to determine the outcome of the promise. The syntax look like this:

```javascript
const myPromise = new Promise((resolve, reject) => {

});
```

A promise has three states `pending`, `fulfilled` and `rejected`. The promise you created is forever stuck in the `pending` state because you did not add a way to complete the promise. The resolve and reject parameters given to the promise argument are used to do this. `resolve` is used when you want your promise to succeed and `reject` is used when you eant it to fail.

Promise are most useful when you have a process that takes an unknown amount of time in your code(i.e something asynchronous), often a server request. When you make a server reques it takes some amount of time, and after it completes you usually want to do something with the response from the server. This can be achieved by using the `then` method. The `then` method is executed immediately after your promise is fulfilled with `resolve`. Here's example - 

```javascript
myPromise.then(result => {
    //do something with the result.
});
```

`catch` is the method used when your promise has been rejected. It is executed immediately after a promise's `rejected` method is called. Here's the syntax:

```javascript
myPromise.catch(error => {
    //do something with the error.
})
```