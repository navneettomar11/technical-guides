# Basic of Bit Manipulation
Working on bytes or data types comprising of bytes like ints, floats, doubles or even data structure which stor large amount of bytes is normal for a programmer. In some cases, a programmer needs to go beyond this- that is to say that in a deeper level where the importance of bits is realized.

Operations with bits are used in **Data Compression** (data is compressed by converting it from one representation to another, to reduce the space), **Exclusive-Or Encryption** (an algorithm to encrypt the data for safety issues). In order to encode, decode or compress files we have extract the data at bit level. Bitwise Operations are faster and closer to the system and sometimes optimize the program to a good level.

We all know that 1 byte comprises of 8 bits and any interger or character can represented usings bits in computers, which we call its binary form(contains only  1 or 0) in its base 2 form.
Example:
14 = {1110}
= 1 x 2^3 + 1 x 2^2 + 1 x 2^1 + 0 x 2^0
= 14

For character we use ASCII representation, which are in the form of integers which again can be represented usings bits as explained above.

## Bitwise Operator
There are different bitwise operations used in the bit manipulation. These bit operations  operate on the individual bits of the bit patterns. Bit operation are fast and can be used in optimizing time complexity. Some common bit operator are:
- **NOT(~)**: Bitwise Not is an unary operator that flips the bits of the number i.e. if the ith bit is 0, it will change it to 1 and vice versa. Bitwise NOT is nothing but simply the one's complement of a number. Lets take an example.
N = 5 = (101)<sub>2</sub>
~N = ~5 = ~(101)<sub>2</sub> = (010)<sub>2</sub> = 2
- **AND(&)**: Bitwise AND is a binary operator that operators on two equal-length bit patterns. If boths bits in the compared position of the bit patterns are 1, the bit in the resulting bit pattern is 1, otherwise 0.
A = 5 = (101)<sub>2</sub>, B = 3 = (011)<sub>2</sub> A & B = (101)<sub>2</sub> & (011)<sub>2</sub> = (001)<sub>2</sub> = 1
- **OR (|)**: Bitwise OR is also a binary operator that operates on two equal-length bit patterns, similar to bitwise AND. If both bits in the compared position of the bit patterns are 0, the bit in the resulting bit pattern is 0, otherwise 1.
A = 5 = (101)<sub>2</sub>, B = 3 = (011)<sub>2</sub>
A | B = (101)<sub>2</sub> |  (011)<sub>2</sub> = (110)<sub>2</sub> = 6
-**XOR(^)**: Bitwise XOR also takes two equal-length bit patterns, If both bits in the compared position of the bit patterns are 0 or 1, the bit in the resulting bit pattern is 0, otherwise 1.
A = 5 = (101)<sub>2</sub>, B = 3 = (011)<sub>2</sub>
A ^ B =  (101)<sub>2</sub> ^  (011)<sub>2</sub> = (110)<sub>2</sub> = 6
- **Left Shift( &lt; &lt; )**: Left shift operator is a binary operator which shift the some number of bits , in the given bit pattern, to the left and append 0 at the end. Left shift is equivalent to multiplying the bit pattern with 2<sup>k</sup> (if we are shifting k bits).
1 << 1 = 2 = 2<sup>1</sup>
1 << 2 = 4 = 2<sup>2</sup>;
1 << 3 = 8 = 2<sup>3</sup>
...
1 << n = 2 <sup>n</sup>
- **Right Shift (>>)**: Right shift operator is binary operator which shift some number of bits, in the given bit pattern, to the right and append 1 at the end. Right shift is equivalent to dividing the bit pattern with 2k(if er are shifting k bits)
4 >> 1 = 2
6 >> 1 = 3
5 >> 1 = 2
16 >> 4 = 1

| X | Y | X & Y | X or Y | X^Y | ~(X)|
|---|---|-------|--------|-----|-----|
| 0 | 0 | 0 | 0 | 0 | 1 |
| 0 | 1 | 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 |

Bitwise operator are good for saving space and sometimes to cleverly remove dependencies. 


### 1. How to check if a given number is a power of 2 ?
Consider a number N and you need to find if N is a power of 2. Simple solution to this problem is to repeated divide N by 2 if N is even. If we end up with a 1 then N is power of 2, otherwise not. There are a special case also. If N = 0 then it is not a power of 2. Let's code it.
**Implementation**
```c++
 bool isPowerOfTwo(int x)
{
	if(x == 0)
		return false;
	else
	{
		while(x % 2 == 0) x /= 2;
		return (x == 1);
		}
}
```
Above function will return true if x is a power of 2, otherwise false.
Time complexity of the above code is O(logN).

The same problem can be solved using bit manipulation. Consider a number x that we need to check for being a power for 2. Now think about the binary representation of (x-1). (x-1) will have all the bits same as x, except for the rightmost 1 in x and all the bits to the right of the rightmost 1.

Let x = 4 = (100)<sup>2</sup>
x - 1 = 3 = (011)<sup>2</sup>

Properties for numbers which are powers of 2, is that they have one and only one bit set in their binary representation. If the number is neither zero nor a power of two, it will have 1 in more than one place. So if x is a power of 2 then x & (x - 1) will be zero.

**Implementation**
```c++
 bool isPowerOfTwo(int x)
{
	// x will check if x == 0 and !(x & (x - 1)) will check if x is a power of 2 or not
	return (x && !(x & (x - 1)));
}
```

### 2. Count the number of ones in the binary representation of the given number.
The basic approach to evaluate the binary form of a number is to traverse on it and count the number of ones. But this approach takes log<sup>2</sup>N of time in every case.
With bitwise operation, we can use an algorithm whose running time depends on the number of ones present in the binary form of the given number. This algorithm is much better as it will reach to logN, only in its worst case.

```c++
int count_one (int n)
{
	while( n )
	{
	n = n&(n-1);
		count++;
	}
	return count;
}
```
As explained in the previous algorithm, the relationship between the bits of x an x-1. So as in x-1, the rightmost 1 and bits right to it are flipped, then by performing x & (x-1), sorting it in x, will reduce x to a number containing number of ones