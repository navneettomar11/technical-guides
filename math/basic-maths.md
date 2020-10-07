# Modular arithmetic
When one number is divided by another, the modulo operation finds the remainder. It is denoted by the % symbol.

**Example**
Assume that you have two numbers 5 and 2. 5%2 is 1 because when 5 is divided by 2, the remainder is 1.

**Properties**
1. (a+b)%c = (a%c + b%c)%c
2. (a*b)%c = ((a%c) * (b%c))%c
3. (a-b)%c = ((a%c) - (b%c) + c)%c
4. (a/b)%c = ((a%c) * (b<sup>-1</sup>%c))%c

> _Note_: In the last property b<sup>-1</sup> is the multiplicative modulo inverse of b and c.

**Example**
If a = 5, b=3 and c=2 then:
- (5+3)%2 = 8%2 = 0
Similary, (5%2 + 3%2)%2 = (1+1)%2 = 0

- (5*3)%2 = 15%2 = 1
Similary, ((5%2) * (3%2))%c = (1*1)%2 = 1

If a = 12, b = 15 and c = 4 then the answer in some languages (12-15)%4 = (12%4 - 15%4)%4 = (0 - 3)%4 = -3. However, the answer of the % operator cannot be negative.
Therefore, to make the answer postive add c to the formula and compute it as follows:
(12-15)%4 = (12%4 - 15%4 + 4)%4 = (0 - 3 + 4)%4 = 1

**When are these properties used**
Assume that a=10<sup>18</sup>, b=10<sup>18</sup> and c = 10<sup>9</sup> + 7
WHen you multiply a with b , the answer is 10<sup>36</sup>, which does not conform with the standard integer data types. Therefore, to avoid this we use properties.
(a*b)%c = ((a%c) * (b%c))%c = (49 * 49)%(10<sup>9</sup> + 7) = 2401

## Congruence Modulo
You may see an expression like:
A≡B (mod C)

This says that A is **congruent** to B modulo C.

## The quotient remainder theorem
When we want to prove some properties about modular arithmetic we often make use of the quotient remainder theorem.
It is a simple idea that comes directly from long divison.
The quotient remainder theorem says:
Given any integer A, and postive integer B, there exist uniqur integer Q and R such that 
A = B * Q + R where 0 <= R <= B

We can see that this come directly from long divsion. When we divide A by B in long divison, Q is the quotient and R is the remainder.
If we can write a number in this form then A mod B = R

# Modular exponentiation
Exponentiation is a mathematical operation is expressed as x<sup>n</sup> and computed as x<sup>n</sup> = x.x....x(n times)
**_Basic Method_**
While calculating x<sup>n</sup>, the most basic solution is broken down into x.x<sup>n-1</sup>. The new problem is x<sup>n-1</sup>, which is similar to the original problem. Therefore, like in original, it is further broken down to x.x.x<sup>n-2</sup>

This is a recursively way of determining the answer to x<sup>n</sup>. However, sometimes an equation cannot be broken down any further as in the case of n  = 0. 
```java
int recursivePower(int x,int n)
{
    if(n==0)
        return 1;
    return x*recursivePower(x,n-1);
}
```
The recusrsive method aligns with explanation, however, the solution can also be written in an iterative format, which is quite ad hac. A variable result to which x is multiplied for n times, is maintained.

The iterative code is as follows:
```java
int iterativePower(int x,int n)
{
    int result=1;
    while(n>0)
    {
        result=result*x;
        n--;
    }
    return result;
}
```
**_Time Complexity_**
With respect to time complexity, it is a fairly efficient O(n) solution. However, when it comes to finding x<sup>n</sup> where n can be as large as 10<sup>18</sup> this solution will not ne suitable.

**_Optimized method**
While calculating  x<sup>n</sup>, the basis of Binary Exponentiation relies on whether n is oldd or even.
If n is even then  x<sup>n</sup> can be broken down to ( x<sup>2</sup>)<sup>n/2</sup>. Programmatically, finding is one-step-process. However, the problem is to find ( x<sup>2</sup>)<sup>n/2</sup>

Notice how computation steps were reduced from n to n/2 in just one step? You can continue to divide the power by 2 aas long as it is even.

When n is odd, try convert it into an even value.  x<sup>n</sup> can be written as  x.x<sup>n-1</sup>. This ensures that n-1 is even
- If n is even replace  x<sup>n</sup> by  (x<sup>2</sup>)<sup>n/2</sup>
- If n is odd replace  x<sup>n</sup> by   x.x<sup>n-1</sup> . n-1 becomes even and you can apply the relevant formula.

This is an efficient method and the _ten-step process_ of determining 3<sup>10</sup> is reduced to a _three-step process_. At every step n is divided by 2. Therefore, the time complexity is O(log N).

However, storing answers that are too large for their respective datatypes is an issue with this method. In some languages the answer will exceed the range of the datatypes while in other languages it will timeout due to large multiplications. In such instances, you must use modulus (%). Instead of finding x<sup>n</sup>. you must find x<sup>n</sup> % m.

For example, run the implementation of the method to find 2<sup>10<sup>9</sup></sup>. The O(n) solution will timeout, while the O(logN) solution will run in time but it will produce garbage values.

To fix this you must use the modulo operation i.e % M in those lines where temporary answer is computed.
```java
int modularExponentiation(int x,int n,int M)
{
    int result=1;
    while(n>0)
    {
        if(n % 2 ==1)
            result=(result * x)%M;
        x=(x*x)%M;
        n=n/2;
    }
    return result;
}
```

## Fast modular exponentiation
**How can we calculate A<sup>B</sup> mod C quickly if B is a power of 2?**

We can use this to calculate 7<sup>256</sup> mod 13 quickly

7<sup>1</sup> mod 13 = 7

7<sup>2</sup> mod 13 = ( 7<sup>1</sup> * 7<sup>1</sup>) mod 13 = (7<sup>1</sup> mod 13 * 7<sup>1</sup> mod 13) mod 13

We can substitute our previous result for 7<sup>1</sup> mod 13 into this equation.

7<sup>2</sup> mod 13 = (7*7) mod 13 = 49 mod 13 = 10

7<sup>4</sup> mod 13    = (7<sup>2</sup> mod 13 * 7<sup>2</sup> mod 13) mod 13
                        = (10 * 10) mod 13 = 100 % 13 = 9

7<sup>8</sup> mod 13    = (7<sup>4</sup> mod 13 * 7<sup>4</sup> mod 13) mod 13
                        = (9*9) mod 13 = 81 mod 13 = 3

We continue in this manner, substituting previous results into our equations.
**....after 5 iteration we hit:**
7<sup>256</sup> = (7<sup>128</sup> mod 13 * 7<sup>128</sup> mod 13) mod 13
                = (3*3) mod 13 = 9

This has given us a method to calculate A<sup>B</sup> mod C quickly provided that **B is a power of 2**.

However, we also need a method for fast modular exponentiation when **B is not power of 2**

i.e 5<sup>117</sup> mod 19

**Step1**: Divide B into powers of 2 by writing it in binary

`117 = 1110101 in binary`

Start at the rightmost digit, let k = 0 and for each digit:
- If the digit is 1, we need a part for 2<sup>k</sup>, otherwise we do not
- Add 1 to k, and move left to the next digit

> 117 = (2<sup>0</sup> + 2<sup>2</sup> + 2<sup>4</sup> + 2<sup>5</sup> + 2<sup>6</sup>)

> 117 = 1 + 4 + 16 + 32 + 64

 5<sup>117</sup> mod 19= 5<sup>(1 + 4 + 16 + 32 + 64)</sup> mod 19 
 5<sup>117</sup> mod 19= (5<sup>1</sup> * 5<sup>4</sup> * 5<sup>16</sup> * 5<sup>32</sup> * 5<sup>64</sup>) mod 19

 **Step 2**: Calculate mod C of the powers of two <= B

 **Step 3**:  Use modular multiplication properties to combine the calculated mod C values




---
**Cryptograms** are puzzles where capital letters stand in for the digits in both places. And, in all of your puzzles, different letter represents different digits.

---
# Prime Number and Composite Number
A prime number (or a prime) is a natural number greate than 1 that cannot be formed by multiplying two smaller natural numbers. A natural number greater than 1 that is not prime is called composite number. For example- 5 is prime because the only ways of writing it as product 1x5 or 5x1, involve 5 itself. However, 6 is composite because it is the product of two numbers (2x3) that are both smaller thant 6.

# Prime sieves
A prime sieve works by creating a list of all integer up to a desired limit and progressively removing composite numbers(which it directly generates) until only primes are left. This the most efficient way to obtain a large range of primes; however to find indvidual primes, direct primality tests are most efficient. Furthermore based on the sieve formalisms, some integer sequences are constructed which also could be used generating prime in certain intervals.

# Large Primes
