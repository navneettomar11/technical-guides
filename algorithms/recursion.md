# Recurisve function
A recursive function is defined in terms of base cases and recursive steps:
- In a base case, we compute the result immediately given the inputs to the function call.
- In a recursive step, we compute the result with the help of one ore more recursive calls to this same function, but with the inputs somehow reduced in size or complexity, closer to a base case.

Consider writing a function to compute factorial. We can define factoriol in two different ways:

**Product**
n! = n x (n-1) x .... x 2 x 1 

```java
    // Iterative
    public static long factorial(int n) {
        long fact = 1;
        for(int i = 1; i <=n; i++) {
            fact *= i;
        }
        return fact;
    }
```

**Recurrence relation**
n! = [
        1 if n = 0, 
        (n-1)!*n if n > 0
    ]

```java
    // Recursive
    public static long factorial(int n) {
        if (n == 0) {
            return 1;
        }
        return n * factorial(n-1);
    }
```

In the recursive implementation on the right, the base case is n = 0, where we compute and return the result immediately: 0! is defined to be 1. The recursive step is n > 0, where we compute the result with help of a recursive call to obtain (n-1)!, then complete the computation by multiplying by n.

## Choosing the Right Decomposition for a Problem
Finding the right way to decompose a problem, such as a method implementation is important, Good decompositions are simple, short easy to understand, safe from bugs and ready for change.

Recursion is an elegant and simple decomposition for some problems. Suppose we want to implement this specification:

```java
/**
 * @param word consisting only of letters A-Z or a-z
 * @return all subsequences of word, separated by commas,
 * where a subsequence is a string of letters found in word 
 * in the same order that they appear in word.
*/
public static String subsequences(String word) {...} 
```

For example, `subsequences("abc)` might return `abc, ab, bc, ac, a, b, c`. Note the trailing comma preceeding the empty subsequence, which is also valid subsequence.
