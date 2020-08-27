# In place Algorithm
An in-place algorithm is an algorithm that does not need an extra space and produces an output in the same memory that contains the data by transforming the input `in-place`. However, a small constant extra space used for variables is allowed.

An in-place algorithm required only a fixed number of interest for auxillary variables i, j and temp, irrespective of size of the input. This can be done by reading the elements from both ends of the array and swapping them as shown below:
```java
public static void inPlaceReverse(int[] A) {
    for(int i = 0, j = A.length - 1; i < j; i++, j--) {
        int temp = A[i];
        A[i] = A[j]
        A[j] = temp;
    }
}
```

# Hamming distance
In informtion theory, the Hamming distance between two strings of equal length is the number of position at which the corresponding symbols are different. In other words, it measure the minimum number of substitutions required to chage one string into other, or the minimum number of errors that could have transformed one string into the other. 

A major appication is in coding theory, more specifically to block codes, in which the equal-length strings are vectors over a finite field.

The symbols may be letters, bits or decimal, among other possibilities. For example, the Hammimg distance between:
- **"karolin"** and **"kathrin"** is 3.
- **1011101** and **10010001** is 2.
- **2173896** and **2233796** is 3

> Note: **Coding theory**: is the study of properties of codes and their respective fitness for specific applications. Codes are used for data compression, cryptography, error detection and correction, data transmission and data storage.

```python
def hamming_code(n1, n2) -> int:
    
    x = n1 ^ n2
    bitCount = 0
    while x > 0:
        bitCount+=x & 1
        x = x >> 1

    return bitCount

```

# Power of Set
In mathematics, the power set (or powerset) of any set S is the set of all subsets of S, including the empty set itself

# Clock angle problem
Clock angle problem are typr of mathematical problem which involve finding the angles between the hands of an analog clock.

Clock angle problem relate two different measurements: angles and time. The angle is typically measured in degrees from the mark of number 12 clockwise. The is usually on a 12-hour clock. 

A method to solve such problems is to consider the rate of change of the angle in degree per minute. The hour of a normal 12-hour analogue clock turns 360 in 12 hours (720 mintues) or 0.5 per minute. The minute hand rotates through 360 in 60 minutes or 6 per minute.
### Equation for the angle of the hour hand
 > &Theta;<sub>hr</sub> = 0.5<sup>o</sup> * M<sub>&Sigma;</sub> = 0.5<sup>o</sup> * (60 * H + M)

where:
* &Theta; is the angle  in degree of the hand measured clockwise from the 12
* H is the hour
* M is the minute past the hour
* M<sub>&Sigma;</sub> is the number of minutes since 12'o clock.

### Equation for the angle of the minute hand
> &Theta;<sub>min</sub> = 6<sup>o</sup> * M

### Equation for the angle between the hands
The angle between the hands can be found using the following formula:
> &Delta;&Theta; = | &Theta;<sub>hr</sub> - &Theta;<sub>min</sub>|

# Gray code
Gray code is a binary numeral system where two successive values differ in only one bit.

For example, the sequence of Gray codes for 3-bit numbers is 000, 001, 011, 010, 110, 111, 100, so G(4) = 6

This code was invented by Frank Gray in 1953.

## Finding Gray code.
Let's look at the bits of number n and the bits of numbers G(n). Notice that i-th bit of G(n) equals to 1 only when i-th bit of n equals 1 and i+1 the bit equals 0 or the other way around (i-th bit equals 0 and i + 1-th bit equals 1). Thus G(n) = n ^ ( n >> 1).

## Finding inverse gray code
Given Gray code g, restore the original number n.

We will move from the most significant bits to the least significant ones (the least significant bit has index 1 and the most significant bit has index k). The relation between the bits n<sub>i</sub> of number n and the bits g<sub>i</sub> of number g.

# Two-pointer techinque
The two pointer technique is mostly applicable in sorted arrays where we try to perform a search in O(n). It's a clever optimization that can help reduce time complexity with no added space complexity by utilizing extra pointers to avoid repetitive operations.

Just like Binary Search is an optimization on the number of trails needed to achieve the result, this approach is used for the same benefit.

> The idea here is to iterate two different parts of the array simultaneously to get the answer faster.

## Sliding Window
Sliding window algorithm is used to perform required operation on specific window sixe of given large buffer or array. Window starts from the 1st element and keeps shifiting right by one element. The objective is to find the minimum k number present in each window. This is commonly known as sliding window problem or algorithm.

# Hamming weight
The Hamming weight of a string is the number of symbols that are different from the zero-symbol of the alphabet used. It is thus equivalent to the Hamming distance from all-zero string of the same length. It is also called the population count, popcount, sideways sum or bit summation.

```python
def hamming_weight(n: int) -> int:
    c = 0
    while c < n: 
        n  = n & (n -1)
        c+=1
    return c
```