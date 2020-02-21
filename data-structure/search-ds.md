# Search Algorithms
Not even a single day pass, when we do not have to search for something in our day to day life, car keys, books, pend, mobile charger and what not. Same is the life of a computer, there is so much data stored in it, that whenever a user asks for some data, computer has to search it's memory to look for the data and make it available to the user. 

Well, to search an element in a given array, there are two popular algorithms available:
1. Linear Search
2. Binary Search

## Linear Search
Linear search is a very basic and simple search algorithm. In Linear search, we search an element or value in a given array by traversing the array fron the starting, till the desired element or value is found.

It compares the element to be searched with all the elements present in the array and when the element is matched successfully, it returns the index of the elmenet in the array, else it return `-1`

Linear search is applied on unsorted or unordered lists, when there are fewer element in a list.

The time complexity of Linear Search algorithms is O(n)

### Feature of Linear Search Algorithm
1. It is used for unsorted and unordered small list of elements.
2. It has a time complexity of O(n), which means the time is linearly dependent on the number of elements, which is not bad mot that good too.
3. It as a very simple implementation.

### Implementing Linear Search
Following are the steps of implementation that we will be following:
1. Traverse the array using a `for` loop.
2. In every iteration, compare the `target` value with the current value of the array.
    * If the values match, return the current index of the array.
    * If the values do not match, move on the the next array element.
3. If no match is found, return -1    

## Binary Search
Binary Search is applied on the sorted array or list of large size. It's time complexity of O(log n) makes it very fast as compared to other sorting algorithms. The only limitation is that array or list of elements must be sorted for the binary search algorithm to work on it.

### Implementing Binary Search Algorithm
Following are steps of implementation that we will be following:
1. Start with the middle element:
    * If the target value is equal to the middle element of the array, then return the index of the middle element.
    * If not, then compare the middle element with the target value,
        * If the target value is greater than the number in the middle index, then pick the elements to the right of the middle index, and start with Step 1.
        * If target value is less than the number in the middle index, then pick the elements to the left of the middle index, and start with Step 1.
2. When a match is found, return the index of the element matched.
3. If no match is found, then return -1

### Time Complexity of Binary Search
When we say the time complexity is `log n`, we actually mean `log<sub>2</sub> n`, although the base of the log does not matter in asymptotic  notations, but still to understand this better, we generally consider a base of 2.

Time complexity of Binary Search = O(log n)