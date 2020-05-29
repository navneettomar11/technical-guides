# Collections
There are four collection data types in the Python programming language:

## List
List is a collection which is ordered and changeable. Allow duplicate members.

```python
thislist = ["apple", "banana", "cherry"]
print(thislist)
```

You access the list item by referring to the index number:
```python
thislist = ["apple", "banana", "cherry"]
print(thislist[1])
```

Negative indexing means beginning from the end `-1` refers to the last item, `-2` refers to the second last item etc.
```python
thislist = ["apple", "banana", "cherry"]
print(thislist[-1])
``` 
You can specify a range of indexes by specifying where to start and where to end the range.

When specifying a range, the return value will be a new list with the specified items.

```python
thislist = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]
print(thislist[2:5]) #['cherry', 'orange', 'kiwi']
```
> Note: The search will start at index 2(included) and end at index (not included).
> Remember that the first item has index 0.

But leaving out the start value, the range will start at the first item:
```python
thislist = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]
print(thislist[:4]) #['apple', 'banana', 'cherry', 'orange']
```

But leaving out the end value, the range will go on to the end of the list: 
```python
thislist = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]
print(thislist[2:]) #['cherry', 'orange', 'kiwi', 'melon', 'mango']
```

Specify negative indexe if you want to start the search from the end of the list;
```python
thislist = ["apple", "banana", "cherry", "orange", "kiwi", "melon", "mango"]
print(thislist[-4:-1]) #['orange', 'kiwi', 'melon']
```

## Tuples
A tuple is a collection which is ordered and unchangeable. In Python tuples are written with round brackets.

### Change Tuple Values
Once a tuple is created, you cannot change it values. Tuples are unchangeable, or immutable as it also is called. But there is a workaround. You can convert the tuple into a list, change the list and convert the list back into a tuple.

```python
x = ("apple", "banana", "cherry")
y = list(x)
y[1] = "kiwi"
x = tuple(y)
```

## Sets
A set is collection which is unordered and unindexed. In Python set are written with curly brackets

You cannot access items in a set by referring to an index, since set are unordered the items has no index.

But you can loop through the set items using a `for` loop or ask if a specified value is present in a set, bby using the `in` keyword.

Once set is created, you cannot change its items, but you can add new items. To add one iten to a set use the `add()` method. To add more than one item to set use the `update()` method.

```python
thisset = {"apple", "banana", "cherry"}
thisset.add("orange")
print(thisset)
thisset.update(["orange", "mango", "grapes"])
print(thisset)
```
To remove an item in a set, use the `remove()` or the `discard` method.
```python
thisset = {"apple", "banana", "cherry"}
thisset.remove("banana")
print(thisset)
thisset.discard("cherry")
print(thisset)
```
> Note: If the item to remove does not exist, `remove()` will raise an error.

## Dictionary
A dictionary is a collection which is unordered, changeable and indexed. In Python dictionaries are with curly brackets, andt they have keys and values.

```python
thisdict = {
  "brand": "Ford",
  "model": "Mustang",
  "year": 1964
}
print(thisdict)
```
You can access the items of a dictionary by referring to its key name, inside square brackets:
> x = thisdict["model"]

The is also a method called `get()` that will give you the same result:
> x = thisdict.get("model")

