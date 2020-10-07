# for...of vs for...in

# Scope
Scope in Javascript refers to the current context of code, which determines the accessibility of variables to Javascript. The two type of scope are local and global.
- **Global variables** are those  declared outside of a block.
- **Local variables** are those declared inside of a block.

|Keyword|Scope|Hoisting|Can be reassigned| Can be redeclared|
|-------|-----|--------|-----------------|------------------|
| **var**| Function scope| Yes| Yes| Yes|
|**let**| Block scope| No | Yes| No|
|**const**| Block scope| No | Ni | No|

# Hositing
In most of the examples so far we've used `var` to declare a variable and we have initialized it with a value. After declaring and initializing, we can access or reassign the variable.

If we attempt to use a variable before it has been declared and initialized, it will return `undefined`.

```javascript
//Attempt to use a variable before declaring it.
console.log(x); //Output = undefined.

//Variable assignment
var x = 100; 
```
However, if we omit the `var` keyword, we are no longer declaring the variable, only initializing it, It will return a `ReferenceError` and halt the execution of the script.

The reason for this is due to hoisting a behavior of Javascript in which variable and function declarations are moved to the top of their scope. Since only the actual declaration is hosited, not the initialization, the value in the first example returns `undefined`.

To demonstrate this concept more clearly, below is the code we wrote and how Javascript actually interpreted it.

```javascript
// the code we wrote
console.log(x);
var x = 100;

// How Javscript interpreted it.
var x;
console.log(x);
x = 100;
```

Javascript saved `x` to memory as a variable before the execution of the script. Since it was still called before it was defined, the result is `undefined` and not `100`. However, it does not cause a `ReferenceError` and halt the script. Although the `var` keyword did not actually change location of the `var`, this is a helpful representation of how hoisting works. This behavior can cause issues, though, because the programmer who wrote this code likely expects the output to `x` to be `true`. when it is instead `undefined`.

# Function declaration and Function expression
The `function` keyword can be used to define a function inside an expression.

A function expression is very similar to and has almost the same syntax as a function declaration. The main difference between a function expression and function declaration is the function name, which can be omitted in function expression to create anonymous function expression can br used as an IIFE(Immediately invoked function  expression) which run as soon as it is defined. 

> Note: Function expression in Javascript are not hoisted, unlike function declarations.

## Name function expression
If you want to refer to current function inside the function body, you need to create a named function expression. This name is then local only to the function body(scope). This also avoid using the non-standards `arguments.calleee` property.
```javascript
let math = {
  'factit': function factorial(n) {
    console.log(n)
    if (n <= 1) {
      return 1;
    }
    return n * factorial(n - 1);
  }
};

math.factit(3) //3;2;1;
```
> Names are useful. Names can be seen in stack traces, call stacks, list of breakpoints, etc, Names are a Good Thing.

# Closure
The lexical scoping, where the scope and value of a variable is determined by whether it is defined/created.

A closure is the combnation of function bundled together(enclosed) with references to its surrounding state(the lexical environment). In other words, a closure gives you access to an outer function's scope from an inner function. In Javascript, closures are created every time a function is create, at function creation time.

```javascript
function makeFunc() {
  var name = 'Mozilla';
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc()
```
The reason is that function in Javascript from closures. A closure is the combination of a function and the lexical environment within which the function was declared. This environment consists of any local variables that were in-scope at the time the closure was created. In this `myFunc` is a reference to the instance of the function `displayName` created when `makeFun` is run. The instance of `displayName` maintain a reference to its lexical environment, within which the variable `name` exists. For this reason, when `myFunc` is invoked, the variable `name` remains avaiable for use, and "Mozilla" is passed to `alert`.

## Pratical closures
Closures are useful because they let you associate data(the lexical environment) with a function operates on that data. This has obvius parallels to object-oriented programming, where objects allow you to associate data(the object properties) with one or more methods.

Consequently, you can use a closure anywhere that you might normally use an object with only a single method.

Situation, where you muight want to do this particularly common on the web. Much of the code-written in front-end javascript is event-based. You define some behavior, and then attach it to an event that is triggered by the user (such as a click or keypress). The code is attached as a callback(a single function that is executed in response to the event).

For instance, suppose we want to add buttons to a page to adjust the text size. One way of doing this is to specify the font-size of the `body` element (in pixels), and then set the size of the other elements on the page (such as headers) using the relative `em` unit.

```css
body {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 12px;
}

h1 {
  font-size: 1.5em;
}

h2 {
  font-size: 1.2em;
}
```
Such interactive text size buttons can change the `font-size` property of the `body` element, and adjustments are picked up by other elements on the page thanks to the relative units.

Here's the Javascript:
```javascript
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);
```
`size12`, `size14`, and `size16` are now functions that resize the body text to 12, 14, and 16 pixels respectively. You can attach them to buttons (in this case hyperlinks) as demostrated in the following code example:
```javascript
document.getElementById('size-12').onclick = size12;
document.getElementById('size-14').onclick = size14;
document.getElementById('size-16').onclick = size16;
```
```html
<a href="#" id="size-12">12</a>
<a href="#" id="size-14">14</a>
<a href="#" id="size-16">16</a>
```
## Emulating private methods with closures
Languages such as Java allow you to declare methods as private, meaning that they can be called only by other methods in the same class.

Javascript does not provided a native way of doing this, but it possible to emulate private method using closures. Private methods aren't just useful for restricting access to code. They also provide a powerful way of managing your global namespace.

The following code illustrate how to use closures to define public functions that can access private functions and variables. Note that these closurese follow the `Module Design pattern`.
```javascript
var counter = (function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }

  return {
    increment: function() {
      changeBy(1);
    },

    decrement: function() {
      changeBy(-1);
    },

    value: function() {
      return privateCounter;
    }
  };
})();

console.log(counter.value());  // 0.

counter.increment();
counter.increment();
console.log(counter.value());  // 2.

```


# Async functions - making promise friendly
Async functions are enabled by default in Chrome 55 and they're quite frankly marvelous. They allow you to write promise-based code as if it were synchronous, but with blocking the main thread. They make your asynchronous code less "clever" and more readable.
Async functions work like this:
```javascript
async function myFirstAsyncFunction() {
    try{
        const fullfilledValue = await promise;
    }catch(rejectedValue) {
        //...
    }
} 
```
If you use the `async` keyword before a function definition, you can then use `await` within the function. When you `await` a promise, the function is paused in a non-blocking way until the promise settles. If the promise fulfills, you get the value back. If the promise rejects, the rejected value is thrown.

## Example: Logging a fetch
Say we wanted to fetch a URL and log the response as text. Here's how it looks using promises:
```javascript
function logFetch(url) {
    return fetch(url)
    .then( response => response.text())
    .then( text => {
        console.log(text);
    }).catch( err => {
        console.error('fetch failed', err);
    });
}
```
And here's the same thing using async functions:

```javascript
async function logFetch(url) {
    try {
        const response = await fetch(url);
        console.log(await response.text();)
    }catch(err) {
        console.log('fetch failed', err);
    }
}
```

It's the same number of lines, but all the callbacks are gone. This makes it way easier to read, especially for those less familiar with promises.

## Async return values
Async functions always return a promise, whether you ise await or not. That promise resolves with whatever the async function returns, or reject with whatever the async function throws. So with: 

```javascript
// wait ms milliseconds
function wait(ms) {
    return new Promise(r => setTimeout(r, ms));
}

async function hello() {
    await wait(500);
    return 'world';
}
```
...calling `hello()` return a promise that fullfills with "world"

```javascript
async function foo() {
    await wait(500);
    throw Error('bar');
}
```
...calling foo() return a promise that reject with Error('bar')

## Example: Streaming a response
The benefit of async functions increases in more complex examples. Say we wanted to stream a response while logging out the chunks, and return the final size.

Here it is with promises:
```javascript
function getResponseSize(url) {
    return fetch(url).then(response => {
        const reader = response.body.getReader();
        let total = 0;
        return reader.read().then(function processResult(result){
            if( result.done ) return total;
            const value = result.value;
            total+=value.length;
            console.log('Received chunk', value);
            return reader.read().then(processResult);
        });
    });
}
```
Check me out, Jake "wielder of promises" Archibald. See how I'm calling `processResult` inside itself to set up an asynchronous loop? Writing that made me feel very smart. But like most "smart" code, you have to stare at it for ages to figure out what it's doing, like one of those magic-eye picture from the 90's.

Let's try again with async functions: 
```javascript
async function getResponseSize(url) {
    const response = await fetch(url);
    const reader = await response.body.getReader();
    let result = await reader.read();
    let total = 0;
    while(!result.done) {
        const value = result.value;
        total+=value.length;
        console.log('Rececived Chunk', value);
        //get the next result
        result = await reader.read();
    }
    return total;
}
```

All the "smart" is gone. The asynchronous loop that made me feel so smug is replaces with a trusty boring, while loop. Much better. In future we'll get async iterators. which would replace while loop with a for-of-loop, making it event neater.


# Arrays
JS arrays can contain different type of data at once. 

Arrays are list-like objects whose prototype has methods to perform traversal and mutation operations. Neither the length of a Javascript array nor the types of its element are fixed. Since an array's length can change at any time, and data can be stored at non-contiguous locations in the array. Javascript arrays are not guaranteed  to be dense; this depends on how the programmer chooses to use them. In general, these are convenient characteristics; but if these features are not desirable for your particular use, you might consider using typed arrays.

> Javascript typed arrays are array-like objects that provide a mechanism for reading and writing raw binary dota im memory buffers. As you may already know Array objects grows and shrink dynamically and can have any Javascript value. Javascript engines perform optimizations so that these  array are fast. 

> A dense array allows programmers to efficiently declare an array when the inital values of the array is known beforehand. Dense arrays are supported in all major browser.

Arrays cannot use strings as element indexes (as in an associative array) but must use integers. Setting or accessing via non-integer using `bracket notation (or dot notation)` will not set or retrieve an element from the array list itself, but will set or access a variable associated with tat array's object property collection. The array's object properties and list of array elements are separate and the array's traversal and mutation operation can not be applied to these names properties.

## Releationship between length and numerical properties
A Javascript array's `length` property and numerical properties are connected.

Several of the built-in array methods (e.g. join(), slice(), indexOf(), etc) take into account the value of an array's `length` property when they're called.

Other methods (e.g. push(), splice(), etc) also result in update to an array's `length` property.

Sometimes we need to do operations on those arrays. Then we use some JS method like `slice` and `splice`. You can see below how to declare an array in Javascript
```javascript
    let arrayDefinition = [];
```
Now let's describe another array with different data types. I will use it below in exampeles.
```javascript
let array = [1,2,3,"hello world", 4.12, true];
```

## Static Methods
| Method | Description|Example|
|--------|------------|-------|
| Array.from()| Creates a new Array instance from `arrayLike`, an array-like or iterable object| Array.from('foo') //output - ['f','o','o']|
| Array.isArray()| Return `true` if value is an array, or false otherwise| Array.isArray([1,2,3]) //output - true|
| Array.of()| Creates a new Array instance with a variable number of arguments, regardless of number of type of the arguments| Array.of(1,2,3) //output - [1,2,3]|

## Instance methods

|Method| Description| Example|
|------|------------|--------|
|concat| Return a new array that is this array joined with other arrays and/or values| var arr = [4,5,6].concat([1,2,3])|
|coyWithin| Copies a sequence of array elements within the array| var arr = array1.copyWithin(0, 3, 4)|
|entries|


## Slice()
The `slice()` method copies a given part of an array and return that copied part as a new array. It doesn't change the original array.
```javascript
    array.slice(from, until);
```