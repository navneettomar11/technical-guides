# Dynamic Programming

![Dynamic Programming](assets/6b68f98.png)

The image above says a lot about Dynamic Programming. So, is repeating the things for which you already have the answer, a good thing? A programmer  would disagree. That's what Dynamic Programming is about. To always remember answers to the sub-problem you've already solved.

Let us say that we have a machine, and to determine its state at time **t**, we have certain quantities called state variables. There will be certain times when we have to make a decision which affects the state of the system, which may or may not be known to us in advance. These decisions or changes are equivalent to transformations of state variables. The result of the previous decision help us in choosing the futuer ones.

What do we conculde from this? We need to break up a problem into a series of overlapping sub-problems and build up solutions to larger and larger sub-problems. If you are given a problem, which can be broken down into smaller sub-problems, and these smaller sub-problems can still be broken into smaller ones - and if you manage to find out that there are some over-lapping sub-problems, then you've encountered a DP problem.

Some famous Dynamic Programming algorithms are :
- Unix diff for comparing two files
- Bellman-Ford for shortest path routing in networks
- TeX the  ancestor if LaTeX
- WASP - Winning and Score Predictor.

The core idea of Dynamic Programming is to avoid repeated work by remembering partial results and this concept find it application in a lot of real life situations.

In programming, Dynamic Programming is a powerful technique that allows one to solve different types of problem in time O(n<sup>2</sup>) or O(n<sup>3</sup>) for which a naive approach would take exponential time.

Write down "1+1+1+1+1+1+1+1 = ? " on a sheet of paper

"What's that equal to?"

Counting "Eight!"

**Write down another "1+" on the left**.

"**What about that?**"

"Nine!" How'd you know it was nine so fast?

You just added one more!

So you didn't need to recount because you remembered there were eight! Dynamic Programming is just a fancy way to say remembering stuff to save time later.

## Dynamic Programming and Recursion
Dynamic programming is basically, recursion plus using common sense. What it means that recursion allows you to express the value of a function in terms of other values of that function. Where the common sense tells you that if you implenent your function in a way that the recursive calls are done in advance, and stored for easy access, it will make your programm faster. This is what we call Memoization - it is memorizing the result of some specific states, which can then be later accessed to solve other sub-problems.

**The intuition behind dynamic programming is that we trade space for time** i.e. to say that instead of calculating all the states taking a lot of time but no space, we take up space to store the result of all the sub-problems to save time later.

Let's try to understand this by taking an example of Fibonacci numbers:
Fibonacci(n) = 1  if n = 0
Fibonacci(n) = 1  if n = 1
Fibonacci(n) = Fibonacci(n-1) + Finbonacci(n-2)

So, the first few numbers is this series will be **1,1,2,3,5,8,13,21,...** and so on!

A code for it using pure recursion
```c
int fib(n) {

    if(n < 2 ) {
        return 1;
    }
    return fib(n-1) + fib(n-2);
}
```
Using Dynamic Programming approach with memoization:
```c
void fib() {
    fibresult[0] =1;
    fibresult[1]= 1;
    for(int i = 2; i < n ; i++) {
        fibresult[i] = fibresult[i-1] + fibresult[i-2];
    }
}
```

In the rescursive code, a lot of values are being  recalculated multiple times. We could do good with calculating each unique quality only once. Take a look at the image to understand that how certain values were being recalculate in the recursive way:

![fibonacci series](assets/fib_tree.png)

Majority of the Dynamic Programming problems can be categorized into two types:
1. Optimization problems
2. Combinatorial problems

The optimization problems expect you to select a feasible solution, so that value of the required function is mimimized or maximized. Combinatorial problems expect you to figure out the number of ways to do something or the probability of some event happening.

Every Dynamic programming problem has a schema to be followed:
- Show that the problem, can be broken down into optimal sub-problems.
- Recursively define the value of the solution by expressing it in term of optimal solutions for smaller sub-problems.
- Compute the value of the optimal solution in bottom-up fashion
- Construct an optimal solution form the computed information.

### Bottom up vs Top Down
- **Bottom up** - I'm going to learn programming. Then, I will start practicing. Then, I will start taking part in contests. Then, I'll practice even more and try to improve. After working hard like crazy, I'll be an amazing coder.
- **Top-Down** - I will be an amazing coder. How? I will work hard like crazy. How? I'll practice more and try to improve. How? I'll start taking part in contests Then? I'll practicing. How? I'm going to learn programming.

Not a great example but I hope I got my point across. In Top Down, you start building the big solution right away by explaining how you build it from smaller solutions. In Bottom Up, you start with the small solutions and then build up.

Memoization is very easy to code and might be your first line of approach for while. Though, with dynamic programming, you don't risk blowing stack space, you end up with lots of liberty of when you can throw calculatons away. The downside os that you have to come up with an ordering of a solution which works.

## How to solve a Dynamic Programming Problem?
Dynamic Programming (DP) is a technique that solves some particular type of problems in Polynomial time. Dynamic Programming solutions are faster than exponential brute method and can be easily proved for their correctness. Before we study how to think Dynamically for a problem, we need to learn:
1. Overlapping Subproblems
2. Optimal Substructure Property.
