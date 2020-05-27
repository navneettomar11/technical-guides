# Brute Force Algorithms
The brute-force method is solving a particular problem by checking all the possible cases which is slow.
For example, you are given a sorted numbers in an array and you have a specific value. The brute force method is to make a for loop and iterate through the elements of the array and eventually you will see the digit you are looking for.

Brute force algorithms refers to a programming style that does not include any shortcuts to improve performance, but instead of relies on sheer computing power to try all possibilities until the solution to a problem is found.

In computer science **brute-force search** or **exhaustive search**, also known as **generate and test**, is a very general problem-solving technique and algorithmic paradigm that consists of systematically enumerating all possible candidates for the solution and checking whether each candidate satisfies the problem statement.

A brute-force algorithm to find the divisors of natural number n would enumerate all integers from 1 to n, and check whether each of them divides n without remainder.

A classic example is the travelling salesman problem (TSP). Suppose a salesman needs to visit 10 cities across the country. How does one determine the order in which cities should be visited such that the total distance travel is minimized? The brute force solution is simply to calculate the total distance for every possible route and then select the shortest one. This is not particularly efficient because it is possible to eliminate many possible routes through clever algorithms.
Another example: 5 digit password in worst case scenario would take 10^5 tries to crack.

The time complexity of brute-force is O(n*m). So, if we were to search for a string of 'n' character in a string of 'm' character using brute force, it would take us n*m tries.
