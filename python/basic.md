# Benefits of Python Programming
- Python is a dynamic-type language. It means that you don't need to mention the data type of variable during their declaration. It allows to set variables like var1=101 and var2="You are an engineer". without any error.
- Python supports object oriented programming as you can define classes along with the composition and inheritance. It doesn't use access specifiers like public or private).
- Functions in Python are like first-class objects. It suggest you can assign them to variables, return from other methods and pass as arguments.
- Developing using python is quick but running it is often slower than compiled languaage. Luckily, Python enables to include the "C" language extensions so you can optimize your scripts.
- Python has several usuage like web-based applications, test automation, data modeling, big data analaytics and much more. Alternatively, you can utilize it as a "glue layer" to work with other languages.


# Python Data Types
Variables can store data of different types, and different types can do different things.

Python has the following data types built-in default, in these categories:

|         |    |  
|---------|----|
|Text Type| str|
|Numeric Types| int, float, complex|
|Sequence Types| list, tuple, range|
|Mapping Type| dict|
|Set Types| set, frozenset|
|Boolean Type| bool|
|Binary Types| bytes, bytearray, memoryview|

## Getting the data type
You can get the data type of any object using the `type()` function:
```python
x = 5
print(type(x))
```
## Setting the Data type
In Python, the dta type is set when you assign a value to a variable:

|Example|Data Type|
|-------|---------|
|x = "Hello World"| str|
|x = 20| int|
|x = 20.5| float|
|x = 1j| complex|
|x = ["apple", "banana", "cherry"]| list|
|x = ("apple", "banana", "cherry")| tuple|
|x = range(6)| range|
|x = {"name": "john", "age": 36}| dict|
|x = {"apple", "banana", "cherry"}| set|
|x = frozenset({"apple", "banana", "cherry"})| frozenset|
|x = True| bool|
|x = b"Hello"| byte|
|x = bytearray(5)| bytearray|
|x = memoryview(bytes(5))| memoryview|

# Strings
Like many other popular programming language, strings in Python are arrays of bytes representing unicode characters.

However, Python does not have a character data type, a single character is simply a string with a length of 1.

Square brackets can be used to access element of the string.
```python
a = "Hello World!"
print(a[1]) # e
```
## Slicing
You can return a range of characters by using the slice syntax.

Specifiy the start index and the end index, separated by colon, to return a part of the string.

```python
b = "Hello World!"
print(b[2:5]) #llo
```

Use negative index to start the slice from the end of the string;
```python
b = "Hello World!"
print(b[-5:-2]) #orl
```

|Method|Description|
|------|-----------|
|len() | The `len()` function returns the length of a string|
|strip()| The `strip()` function removes any whitespaces from the beginning or the end|
|lower()| The `lower()` method returns the string in lower case|
|upper()| The `upper()` method returns the string in upper case|
|replace()| The `replace()` method replace a string with another string|
|split()| The `split()` method splits the string into substrings if it find instance of the separator|

### Check String
To check if a certain phrase or character is present in a string, we can use the keywords `in` or `not in`.

### Mutliline 
You can assign a multiline string to a variable by using three quotes
```python
a = """Lorem ipsum dolor sit amet,
consectetur adipiscing elit,
sed do eiusmod tempor incididunt
ut labore et dolore magna aliqua."""
```
or three single quotes
```python
a = '''Lorem ipsum dolor sit amet,
consectetur adipiscing elit,
sed do eiusmod tempor incididunt
ut labore et dolore magna aliqua.'''
```

# The pass statement
`if` statement cannot be empty, but if you for some reason have an `if` statement with no content, put in the `pass` statement to avoid getting an error.

# Function
A function is a block of code which only runs when it is called.

If you do not know how many arguments that will be passed into your function, add a `*` before the parameter name in the function definition.

This way the function will receive a tuple of arguments, and can access items accordingly.
```python
def my_function(*kids):
  print("The youngest child is " + kids[2])

my_function("Emil", "Tobias", "Linus")
```
You can also send arguments with the key = value syntax.

This way the order of the arguments does not matter.

```python
def my_function(child3, child2, child1):
  print("The youngest child is " + child3)

my_function(child1 = "Emil", child2 = "Tobias", child3 = "Linus")
```
If you do not know how many keyword arguments that will be passed into your function, add two `**` before the parameter name in the function definition.

This way the function will receive a `dictionary` of arguments and can access the items accordingly.


## Lambda
A lambda function is a small anonymous function. A lambda function can take any number of arguments, but can only have one expression.

```python
x = lambda a : a  + 10
print(x(5))
```

### Why use Lambda Functions?
Lambda functions are used when you need a function for a short period of time. This is commonly used when you want to pass a function as an argument to higher-order functions, that is functions that take other functions as their arguments.

### What does the if __name__ == "__main__": do?
Whenever the Python iterpreter reads a source file, it does two things:
- it sets a few special variables like __name__, and then
- it executes all of code found in the file.

Let's see how this works and how it relates to your question about __name__ checks we always see in Python script.

## Package
A python package is simply an organized collection of python modules. A python module is simply python file.

Creating a package with `__init__.py` is all ablut making it easier to develop large Python projects.

The `__init__.py` files are required to make Python treat directories containing file as packages. This prevent directories with a common name, such as string, unitentionally hiding valid modules that occur later on the module search path. 

  
