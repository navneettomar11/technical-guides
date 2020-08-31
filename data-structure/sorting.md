# Sorting
Sorting is nothing but arranging the data in ascending or descending order. The term **sorting** came into picture, as humans realised the importance of searching quickly.

## Bubble Sort
Bubble Sort is a simple algorithm which is used to sort a given set of n elements provided in form of an array with n number of elements. Bubble Sort compares all the element one by one and sort them based on their values.

### Implementing Bubble Sort Algorithm
Following are the steps involved in bubble sort(for sorting a given array in ascending order).
1. Starting with the first element (index = 0), compare the current element with the next element of the array.
2. If the current element is greater than the next element of the array, swap them.
3. If the current element is less than the next element, move to the next element. **Repeat 1**

### Complexity Analysis of Bubble Sort
In Bubble sort `n-1` comparisons will be done in the 1st pass, `n-2` in 2nd pass. `n-3` in 3rd pass and so on. So the total number comparisons will be
(n-1)+ (n-2)+ (n-3)+........+3+2+1
Sum = n(n-1)/2
i.e. O(n<sup>2</sup>)

Hence the time complexity of Bubble Sort is O(n<sup>2</sup>)

The main advantage of Bubble Sort is the simplicity of the algorithm
The space complexity for Bubble Sort is O(1), because only a single additional memory space is required i.e. for temp variable.
Also, the best case time complexity will be O(n), it is when this list is already sorted.

## Selection Sort
Selection sort is conceptually the most simplest sorting algorithm. The algorithm will first find the smallest element in the array and swap it with the element in the first position, then it will find the second smallest element and swap it with the element in the second position, and it will keep on doing this until the entire array is sorted.
It is called selection because it repeatedly selects the next smallest element and swap it into the right place.

### Complexity Analysis of Selection Sort
Selection Sort requires two nested `for` loops to complete itself, one `for` loop is in the function and inside the first loop we are making a call to another function, which has the second inner `for` loop.

Hence for a given input size of `n`, following will be the time and space complexity for selection sort algorithm:
Worst Case Time complexity : O(n<sup>2</sup>)
Best Case Time complexity: O(n<sup>2</sup>)
Space Complexity: O(1)

## Insertion Sort
Consider you have 10 cards out of a deck of cards in your hand. And they are sorted, or arranged in the ascending order of their numbers.
If I give you another card, and ask you to insert the card in just the right position so that the cards in your hand are still sorted. What will you do?
Well, you have to go through each card from the starting or the back and find the right position for the new card, comparing it's value with each card. Once you find the right position, you will insert the card there.
Similarly, if more new cards are provided to you, you can easily repeat the same process and insert the new cards and keep the cards sorted too.
This is exactly how insertion sort works. It starts from the index 1(not 0), and each index starting from index 1 is like a new card, that you have to place at the right position in the sorted subarray on the left.
Following are some of the important characteristics of Insertion Sort:
1. It is efficient for smaller data sets. but very inefficient for large lists.
2. Insertion sort is adaptive, that means it reduces its total number of steps if a partially sorted array is provided as input, making it efficient.
3. It is better than Selection Sort and Bubble sort algorithms.
4. Its space complexity is less. Like bubble sort, insertion sort also requires a single additional memory space.
5. It is a stable sorting technique, as it does not change the relative order of elements which are equal.

### Compexity Analysis of Insertion Sort
Worst case time complexity : O(n<sup>2</sup>)
Best case time complexity: O(n)
Space Complexity: O(1)

## Merge Sort
Merge Sort follows the rule of Divide and Conquer to sort a given set of numbers/elements, recursively, hence consuming less time.

In selection and insertion sort, both of have a worst case running time of O(n<sup>2</sup>). As the size of input grows, insertion and selection sort can take a long time to run.

Merge sort, on the other hand, runs in `O(n*log n)` time in all the cases.

In Merge sort, the given unsorted array with n elements, is divided into n subarrays, each have one element, because a single element is always sorted in itself. Then, it repeatedly merge these subarrays to produce new sorted subarrays, and in the end, one complete sorted array is produced.

The concept of **Divide and Conquer** involves three steps:
1. **Divide** the problem into multiple small problems.
2. **Conquer** the subproblems by solving them. The idea is to break down the problem into atomic subproblems, where they are actually solved.
3. **Combine**: the solution of the subproblems to find the solution of the actual problem.

### How Merge Sort Works?
In merge sort, we break the given array midway, for example if the original array had `6` elements, then merge sort will break it down into two subarrays with elements each.
But breaking the original array into 2 smaller subarrays is not helping us in sorting the array.
So we will break these subarrays into event smaller subarrays, until we have multiple subarrays with single element in them. Now, the idea here is that an array with a single element is already sorted , so once we break the original array into subarrays which has only a single element, we have successfully broken down our problem into base problems.
And then we have to merge all these sorted subarrays, step by step to from one single sorted array.

### Complexity Analysis of Merge sort
Merge sort is quite fast ans has a time complexity of O(n*log n).

## Quick Sort Algorithm
Quick Sort is also based on the concept of Divide and Conquer, just like merge sort. But in quick sort all the heavy lifting(major work) is done while dividing the array into subarrays, while in case of merge sort, all the real work happens during merging the subarrays. In case of quick sort, the combine step does absolutely nothing.
It is also called **partition-exchange sort**. This algorithm divides the list into three main parts:
1. Element less than the Pivot element.
2. Pivot element (Central element)
3. Elements greater than the pivot element.

Pivot element can be any element from the array, it can be the first element, the last element or any random element.

### How Quick Sorting Works
Following are the steps involved in quick sort algorithm:
1. After selecting an element as **pivot**, which is the last index of the array in our case. we divide the array for the first time.
2. In quick sort, we call this partitioning. It is not simple breaking down of array into 2 subarrays, but in case of partitioning, the array elements are so positioned that all the element smaller than the pivot will be on the left side of the pivot and all the element greater than the pivot will be on the right side of it.
3. And the pivot element will be as its final sorted position.
4. The elements to the left and right may not be sorted.
5. Then we pick subarrays, elments of the left of pivot and element on the right of pivot, and we perform partitioning on them by choosing a pivot in the subarrays.

### Complexity Analysis of Quick Sort
For an array, in which **partitioning** leads to unbalanced subarrays, to an extend where on the left side there are no elements, with all the elements greater than the **pivot**,hence on the right side.
And if keep on getting unbalanced subarrays, then the running time is the worst case, which is O(n<sup>2</sup>)
Where as if **partitioning** leads to almost equals subarrays, then the running time is the best
, with time complexity as O(n*log n)

Worst case : O(n<sup>2</sup>)
Best case:  O(n*log n)

Space complexity:  O(n*log n)

To avoid this, you can pick random **pivot** element too. If won't make any difference in the algorithm, as all you need to do is, pick a random element from the array, swap it with element at the last index, make it the **pivot** and carry on with quick sort.

## Heap Sort
Heap sort is one of the best sorting methods being in-place and with no quadratic worst-case running time. Heap sort involves building a Heap data structure from the given array and then utilizing the Heap to sort the array.

You must be wondering, how converting an array of numbers into a heap data structure will help in sorting the array. To understand this, let's start by understanding what is Heap?

### What is Heap?
Heap is a special tree-base data structure, that satisfies the following special heap properties:
1. **Shape Property**: Heap data structure is always a Complete Binary Tree which meand all levels of the tree are fully filled.
2. **Heap Property**: All nodes are either greater than or equal or less than or equal to each of its children. If the parent nodes are grater than their child nodes, heap is called a **Max-Heap**, and if the parent nodes are smaller than their child nodes, heap is called **Min-heap**.

### How heap sort works?
Heap sort algorithm is divided into two basic parts:
- Creating a Heap of the unsorted list/array.
- Then a sorted array is created by repeatedly removing the largest/smallest element from the heap, and inserting it into the array. The heap is reconstructed after each removal.

Intially on receiving an unsorted list, the first step in heap sort is to create a Heap data structure(Max Heap or Min Heap). Once heap is built, the first element of the Heap is either largest or smallest(depending upon Max-Heap or Min-Heap), so we put the first element of the heap in our array. Then we again make heap using the remaining elements, to again pick the first element of the heap and put it into the array. We keep on doing the same repeatedly until we have the completed sorted list in our array.

### Complexity Analysis of Heap Sort
Worst case time Complexity: O(n*log n)
Best case Time complexity: O(n*log n)
Space Complexity: O(1)

## Counting Sort
In counting sort, the frequencues of distinct elements of the array to be sorted is counted and stored in an auxiliary array, by mapping its value as an index of the auxiliary array.

**Algorithm**
Let's assume that, array A of size N needs to be sorted.
- Intialize the auxiliary array Aux[] as 0.
Note: The size of this array should be >= max(A[])
- Traverse array A and store the count of occurrence of each element in the appropriate index of the Aux array, which means execute Aux[A[i]]++ for each i, where i ranges from [0, N-1].
- Intialize the empty array sorted A[]
- Traverse array Aux and copy i into sortedA for Aux[i] number of times where 0 <= i <= max(A[]).

Note: The array can be sorted by using this algorithm only if the maximun value in array A is less than the maximum size of the array Aux. Usually, it is possible to allocate memory up to the order of million (10<sup>6</sup>). If the maximum value of A exceeds the maximum memory-allocation size, it is recommended that you do not use this algorithm. Use either the quick sort or merge sort algorithm.

### Time Complexity
The array A is traversed in O(N) time and the resulting sorted array is also computed in O(N) time . Aux[] is traversed in O(K) time. Therefore, the overall time complexity of counting sort algorithm is O(N + K).

## Radix Sort
QuickSort, MergeSort, HeapSort are comparison based sorting algorithms. 
CountSort is not comparison based algorithm. It has the complexity of O(n+k), where k is the maximum element of the input array.
So, if K is O(n), CountSort becomes linear sorting, which better than comparison based sorting algorithm thqt have O(n*log n) time complexity. The idea is to extend the CountSort algorithm to get a better time complexity when k goes O(n<sup>2</sup>). Here comes the idea of Radix sort.

For each digit i where i varies from the least significant digit to the most significant digit of a number. Sort input array using countsort algorithm according to ith digit.

### Complexity Analysis
THe complexity is O((n+b) * log<sub>b</sub> (maxxx)) where b is the base for representing numbers and maxxx is the maximum element of the input array. This is clearly visible as we make (n + b) iterations log<sub>b</sub>(maxx) times (number of digits in the maximum element). If maxx <= n <sup>c</sup>, then the complexity can be written as O(n * log<sub>b</sub>(n)).

### Advantages
1. Fast whene the keys are short i.e. when the range of the array element is less.
2. Used in suffix array construction algorithms like Manber's algorithm and DC3 algorithm.

### Disadvantage
1. Since Radxi Sort depends on digits or letters, Radix Sort is much less flexible than other sorts. Hence, for every different type of data it needs to be rewritten.
2. The constant for Radix sort is greater compared to other sorting algorithms.
3. It takes more space compared to Quicksort which inplace sorting.

## Bucket Sort
Bucket sort is a comparison sort algorithm that operates on elements by dividing them into dividing them into different buckets and then sorting these buckets individually. Each bucket is sorted individually using a separate sorting algorithm or by applying the bucket sort algorithm recursively. Bucket sort is mainly useful when the input is uniformally distributed over a range.

1. Create n empty buckets (or lists)
2. Do following for every array element arr[i]
    * Insert arr[i] into bucket[n * arr[i]]
3. Sort individual bucket using insertion sort.
4. Concatenate all sorted buckets.

### Time Complexity
If we assume that insertion in a bukcet takes O(1) time then steps 1 and 2 of the above algorithm clearly take O(n) time. The O(1) is easily possible if we used a linked list to represent a bucket.


## Wave Sort


## Stalin Sort
Stalin sort(also 'dictor sort' and 'trump sort') is a nonsensical sorting algorithm in which each element that is not in the correct order is simply eliminated from the list.

1. Gather up the comrades and show them the list.
2. Ask the comrades to raise their hands if they believe the list is sorted.
3. Select any comrades who did not raise his hand, and executes him as a traitor.
4. Repeat steps 2 and 3 until everyone agrees that list is sorted.

## Cycle Sort
Cycle sort is a comparison sorting algorithm which forces array to be factored into the number of cycles where each of them can be rotated to produce a sorted array. It is theoretically optimal in the sense that it reduces the number of writes to the original array.


# Topological Sorting
