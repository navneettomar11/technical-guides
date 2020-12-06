# Two pointer technique

The 2 pointer technique is mostly applicable in sorted arrays where we try to perform a search in O(N). While it does not refer to situation such as searching a pair of items in an array that sums up to a given target value, then two nested for loops are needed to search all the possible pairs. These are different ways to put these two pointers:
1. **Equi-directional**: Both start from beginning: we have slow-faster pointer, sliding window algorithm.
2. **Opposite-directional**: One at the start and the other at end, they move close to each other ad meet in the middle. 

Here's an example.
Given two arrays (A and B) sorted in ascending order and an integer X, we need to find i and j such that a[i] + b[i] is equal to X.

i and j are our pointers, at first step i is pointing to the first element of a and j points to the last element of b.

```java
i =0; j = b.size() - 1
```
Move the first pointer one by one through the array a, and the correct position of the second pointer if needed
```java
while (i < a.size()) {
	while(a[i] + b[j] > X && j > 0 ) j--;
	if(a[i] + b[j] == X) writeAnswer(i, j)
	i++;
}
```

## Window Sliding Techinque
The technique can be best understood the window pane in bus, consider a window of length n and the page which is fixed in it of length k. Consider, intially the pane is at extreme left i.e. at 0 units from the left. Now co-relate the window with array arr[] of size n and plane with current_sum of size k elements. Now if we apply force on the window such that it moves a unit distance ahead. The pane will cover next k consecutive elements.