# PEP 8
PEP 8 is the latest Python coding standard, a set of coding recommendations. It guides to deliver more readable the Python code.

# What is the output of the following Python code fragment? Justify your answer
```python
def extendList(val, list=[]):
    list.append(val)
    return list

list1 = extendList(10)
list2 = extendList(123,[])
list3 = extendList('a')

print "list1 = %s" % list1
print "list2 = %s" % list2
print "list3 = %s" % list3
```
The result of the above Python code snippet is:
```bash
list1 = [10, 'a']
list2 = [123]
list3 = [10, 'a']
```

You may erroneously expect list1 to be equal to [10] and list3 to match with ['a'], thinking that the list argument will initialize to its default value of every of [] every time there is a call to the the extendList.

However, the flow is like that a new list gets created once after the function is defined. And the same get used whenever some calls the extendList method without a list argument. It work like this because the calculation of expression(in default expression) occurs at the time of function definition, not during its invocation.

The list1 and list3 are hence operating on the same default list, whereas list2 running on separate object that it has created on its own(by passing an empty list as the value if the list parameter). 

# How to find bugs or perform static analysis in a Python application?
- You can use PyChecker, which is a static analyzer. It identifies the bugs in Python project also reveals the style and complexity related bugs.
- Another tool is PyLint, which checks whether the Python modules satisfies the coding standard.

# How does Python handle memory management
- Python use private heaps to maintain its memory. So the heap holds all the Python objects and the data structures. This area is only accessible to the Python interpreter; programmers can't use it.
- And it' the Python memory manager that handles the private heap. It does the required allocation of the memory for Python objects.
- Python employs a built-in garbage collector, which salvages all the unused memory and offloads it to the heap space.

# What are the principal differences between the lambda and def?
- Def can hold multiple expressions while lambda is a uni-expression function.
- Def generates a function and designates a name to call it later. Lambda forms a function object and return it.
- Def can have a return state. Lambda can't have return statements.
- Lambda supports to get used inside a list and dictionary.

# Range Function
Range() generate a list of numbers, which is used to iterate over for loops.

```java
for i in range(5):
    print(i)
```
The range() function accompanies two sets of parameters.
- range([start], stop[, step])
    - start: It is the starting no. of the sequence.
    - Stop: It specifies the upper limit of the sequence.
    - Step: It is the incrementing factor for generating the sequence.
**Points to note**
- Only integer arguments are allowed
- Parameters can be postive or negative.
- The **range()** function in Python starts from the zeroth index.

# What are the optional statements possible inside a try-except block in Python?
There are two optional clauses you ca use in the **try-except** block.
- The **"else"** clause - It is useful if you want to run a piece of code when the try block doesn't create an exception.
- The **finally** clause - It is useful when you want to execute some steps which run, irrespective of whether there occurs an exception or not.


# https://www.techbeamers.com/python-interview-questions-programmers/
# https://www.techbeamers.com/python-programming-quiz-for-beginners-part-1/

# When is the Python decorator used?