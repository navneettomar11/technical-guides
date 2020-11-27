
# Closures
A **closure** is the combination of a function bundled together(enclosed) with references to its surrounding state(the lexical environment).  
A closure is a special kind of object that combines two things: a function, and the environment in which that function was created. The environment consists of any local variables that were in-scope at the time that closure was created.

## Lexical Scoping
A Lexical scope in Javascript means that a variable defined outside a function can be accessible inside another function defined after the variable declaration. In simple word, this means the child scope has access to the variable defined in the parent's scope. 


# Spread syntax(...)
Spread syntax(...) allows an iterable such as array expression or string to be expanded in places where zero or more arguments(for function calls) or elements(for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs(for object literals) are expected.


References:
- [Execution context, scope chain and Javascript internals](https://medium.com/@happymishra66/execution-context-in-javascript-319dd72e8e2c)
- [Understanding Execution Context and Execution Stack in Javascript](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)