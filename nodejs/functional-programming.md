# Functional Programming
Functional programming is a style of programming where solutions are simple, isolated functions, without any side effects outside of the function scope.

> INPUT -> PROCESS -> OUTPUT

Functional programming is about:
1. **Isolated functions** - there is no dependencies on the state of the program, which includes global variables that are subject to change.

2. **Pure functions** - the same in input always gives the same output.

3. Function with limited side effects - any changes, or mutations, to the state of the program outside the function are carefully controlled.

**Callbacks** are the functions that are slipped or passed into another function to decide the invoction of that function. You may have seen them passed to other methods, for example in `filter`, the callback function tells Javascript the criteria for how to filter an array.

Functions that can be assigned to a variable, passed into another function, or returned from another function just like any other normal value, are called first class functions. In javascrit, all functions are first class functions.

The function that take a function as an argument, or return a function as a return value are called higher order functions.

When the functions are passed in to another function or returned from another function, then those function which gets passed in or returned can be called a lambda.

One of the core principles of functioncal programming is to not change things. Changes lead to bugs. It's easier to prevent bugs knowing that your functions don't change anything including the function arguments or any global variable.

Recall that is functional programming, changing or altering things is called mutation, and the outcome is called a side effect. A function, ideally, should be a pure function, meaning that it does not cause any side effects.

So far, we have seen two distinct principles for functional programming:

1) Don't alter a variable or object-create new variable and objects and return them if need be from a function.

2) Delcare function arguments - any computation inside a function depends only on the argumenets, and not on any global object or variable.

The arity of a function is the number of arguments it requires. Curring a function means to convert function of N arity into N functions of arity 1.

In other words, it restructure a function so it takes one argument, then returns another function that takes the next argument, and so on.

Here's an example -
```javascript
//Un-curried function
function unCurried(x, y) {
  return x + y;
}

//Curried function
function curried(x) {
  return function(y) {
    return x + y;
  }
}

//Alternative using ES6
const curried = x => y => x + y

curried(1)(2) // Returns 3
```

This is useful in your program if you can't supply all the arguments to a function at one time. You can save each function call into a variable, which will hold the returned function reference that takes the next argument when it's available. Here's an example using the curried function in the example above:

```javascript
// Call a curried function in parts:
var funcForY = curried(1);
console.log(funcForY(2)); // Prints 3
```

Similarly, partial application can be described as applying a few argument to a function at a time and returning another function that is applied to more arguments. Here an example:

```javascript
//Impartial function
function impartial(x, y, z) {
  return x + y + z;
}
var partialFn = impartial.bind(this, 1, 2);
partialFn(10); // Returns 13
```

# Pure functions
A pure function doesn't depend on and doesn't modify the states of variables out of its scope.

Concretely, that means a pure function always returns the same result given same parameters. Its execution doesn't dependt of the system.

For example
```javascript
var values = {a: 1};

function impureFunction(items) {
  var b = 1;
  items.a = items.a * b + 2;

  return items.a;
}

var c = impureFunction(values);
//Now `values.a` is 3, the impure function modifies it.
```

Here we modify the attributes of the given object. Hence we modify the object which lies outside of the scope of our function: the function is impure.

```javascript
function pureFunction(a) {
  var b = 1;
  a = a * b + 2
  return a
}
var c = pureFunction(values.a)
// `values.a` has not been modified, it's still 1.
```

## Advantages of pure functions
The main advantage of a pure function is that it doesn't have any side effect. It doesn't modify the state of the system outside of their scope. Then, they just simply and clarify the code: when you call a pure function, you just need to focus on the return values as you know you didn't broke anything elsewhere doing so.

A pure function is also robust. Its order of execution doesn't have any impact on the system. Operations with pure function could be parallelized.

Also, it very easy to unit test a pure function, since there is not context to consider. Just focus on inputs / outputs.

Finally, maximizing the use of pure functions make your code simpler, more flexible.
