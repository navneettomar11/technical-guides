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
As explained in the previous algorithm, the relationship between the bits of x an x-1. So as in x-1, the rightmost 1 and bits right to it are flipped, then by performing x & (x-1), sorting it in x, will reduce x to a number containing number of ones.


# XOR operator
The XOR logical operation, or exclusive or takes two boolean operand and return true if and only if the operand are different. Thus, it return false if the two operand have the same value.

So, the XOR operator can be used, for example, when we have to check for two conditions that can't be true at the same time.

Let's consider two condition A and B. Then the following table shows the possible values of A XOR B

|A|B|A XOR B|
|-|-|-------|
|T|T|F|
|T|F|T|
|F|T|T|
|F|F|F|

**The A XOR B operation is equivalent to (A && !B) || (!A && B).** Parentheses have been included for clarity, but are optional as the AND operator takes precedence over the OR operator.

# Mathematical Operation using Bitwise operators

## Addition
Sum of two bits can be obtained by performing XOR(^) of two bits. Carry bit can be obtained by performing AND (&) of two bits.

Above is simple `Half Adder` logic that can be used to add 2 single bits. We can extends this logic for integers. If x and y don'y have set bits at same position(s), then bitwise XOR(^) of x and y gives the sum of x and y. To incorporate common set bits also, bitwise AND(&) is used. Bitwise AND of x and y gives all carry bits. We calculate (x&y) << 1 and add it to x^y to get the required result.

```python
#(Iterative)
def add(x, y) -> int:
	while y != 0:
		carry = x & y
		x = x^y
		y = carry << 1

	return x

def add_recursive(x, y) :
	if y == 0:
		return 0
	else:
		return 	add_recursive(x^y, (x & y) << 1)

```

## Substraction
Like addition , this idea us ti user `subtractor` logic.

```python
def subtract(x, y) -> int:
	while y != 0:
		borrow = not x & y
		x = x ^ y
		y = borrow << 1
	return x
```

## Multiplication (Russian Peasant)
The idea is to double the first number and halve the second number repeatedly till the second number doesn't become 1. In the process, whenever the second number become odd, we add the first number to result.

```python
def multiply(a,b):
	res = 0
	while b != 0:
		if b & 1 :
			res+=a

		a = a << 1
		b = b >> 1

	return res
```

## Divison
Q = N/D
1. Align the most-significant ones on N and D. (for checking sign)
2. Compute quoitent t = N - D
3. if t >= 0 then set the least significant bit to Q to 1, and set N = t
4. Left shift N by 1
5. Left shift Q by 1
6. Go to step 2

```python
	def divide(n, d) -> int:
		sign = -1 if n * d <  0 else 1
		q=1
		rq = 0
		while True:
			t  = d << 1
			if n >= t:
				d = t
				q+=1
			else:
				rq+=q
				q = q
				n-=d
				if d << 1 > n:
					if n >= d:
						rq+=1
					break		
		return sign * rq
```

## Odd or Even using Bitwise Operator
1. **Using Bitwise XOR operator** - The idea to check whether last bit of the number is set or not. If last bit is set then the number is odd otherwise even.
	As we know bitwise XOR operation of the Number by 1 increment the value of the number by 1 if the number is even otherwise it decrements the valude of the number by 1 if the value is odd.
	```python
		def is_even(x):
			return x^1 < x
	```
2. **Using Bitwise AND operator** - The idea is to check whether the last bit of the number is set or not. If last bit is set then the number is odd, otherwise even.
	As we know bitwise AND operation of the Number by 1 will be 1, If it is odd because the last bit will be already set. Otherwise it will give 0 as output.
	```python
		def is_even(x):
			return x & 1 != 1
	```
# Bit Manipulation Tactics
1. We can quickly calculate the total number of combinations with numbers smaller than or equal to width a number whose sum and XOR are equal. Instead of using looping, we can directly find it by mathematical trick.
	> Answer = pow(2, count of zero bits)

2. If a number is a power of 2.
	```python
	def is_power_two(x: int) -> bool:
		return x && ( x & (x-1))
	```	
3. We can find the most significant set bit in O(1) time for a fixed size integer. For example below code is for 32-bit integer
	```python
		def set_bit_number(n):
			# Suppose n is 273(binary is 100010001). It does following 100010001 | 010001000 = 110011001 
			n |= n >> 1
			# This makes sure 4 bits. (From MSB and including MSB) are set. It does following 110011001 | 001100110 = 111111111 
			n |= n >> 2
			n |= n >> 4
			n |= n >> 8
			n |= n >> 16
			# Increment n by I so that there is only one set bit which is just before orginal MSB. n now becomes 1000000000
			n+=1
			# Return original MSB after shifting n now becomes 100000000 
			return n >> 1
	```

4. **Check if the i<sup>th</sup> bit is et in the binary form of the given number.**

	To check if the i<sup>th</sup> bit is set or not(1 or not), we use AND operator. How?

	Let's say we have a number N, and check whether it's i<sup>th</sup> is set or not, we can AND it  with the number 2<sup>i</sup>. The binary form of 2<sup>i</sup> contains only the i<sup>th</sup> bit as set (or 1) else every bit is 0 there. When we will AND it with N and if the i<sup>th</sup> bit of N is set, then it will return a non zero number (2<sup>i</sup> to be specific) else 0 will be returned.

	Using left shift operator, we can write 2<sup>i</sup> << i. Therefore:
	```python
		def check(int n) -> bool:
			if n & (1 << i) :
				return True
			else:
				return False	
	```

	**Example**: Let's say N = 20 = (10100)<sub>2</sub>. Now let's check if it's 2nd bit is set or not(starting from 0). For that we have to AND it with 2<sup>2</sup> = 1 << 2 = {100}<sub>2</sub>. {10100} & {100} = {100} = 2<sup>2</sup> = 4 (non-zero element), which means its 2nd bit is set.

5. **How to generate all the possible subsets of a set ?**

	A big advantage of bit manipulation is that it can help to iterate over all the subsets of an N-element set. As we  all know there are 2<sup>N</sup> possible subsets of any given set with N elements. What if we represent each element in a subset with a bit. A bit can be either 0 or 1, thus we can use this to denote whether the corresponding element belong to this given subset or not. So each bit pattern will represent a subset.

	Consider a set A of 3 elements:

	A = {a, b, c}
	
	Now, we need 3 bits, one bit for each element. 1 represet that correponding element is present in the subset, whereas 0 represent the corresponding element is not in the subset. Let's write all the possible combination of these 3 bits.

	0 = (000) = {}

	1 = (001) = {c}

	2 = (010) = {b}

	3 = (011) = {b.c}

	4 = (100) = {a}

	5 = (101) = {a,c}

	6 = (110) = {a,b}

	7 = (111) = {a,b,c}

	```python
		def possible_subset(s:str) -> List[str]:
			n = len(s)
			res = []
			for i in range(0, 1<< n):
				subset = []
				for  j in range(0, n):
					if i & (1 << j):
						print('I = ',i,' J = ', j, 'cond ', (i & (1 << j)), ' s[j] = ', s[j])
						subset.append(s[j])

				res.append(subset)

			return res
	```

	6. **Find the largest power of 2 (most significant bit in binary form), which is less than or equal to the given number N.
		
		**Idea**: Change all the bits which are at the right side of the most significant digit to 1.
		
		**Property**: As we know that when all the bits of a number N are 1, then N must be equal to the 2<sup>i</sup>-1, wheere i the number of bits N. 

		**Example**: 

		Let say binary form of N is {1111}<sub>2</sub> which is equal to 15. 15 = 2<sup>4</sup>-1, where 4 is the number of bits in N.

		This property can be used to fid the larged power of 2 less than or equal to N. How ?

		If we somehow change all the bits which are at right side of the most significant bit of N to 1. then the number will become x + (x-1) = 2*x-1, where x is the required number.

		```python
			def largest_power(n): 
				n = n | (n >> 1)
				n = n | (n >> 2)
				n = n | (n >> 4)
				n = n | (n >> 8)

				return (n+1) >> 1
		```

# Sieve of Eratosthenes
In mathematics, the sieve of eratosthenes is an ancient algorithm for finding all prime numbers up to any given limit.

It does so by iteratively marking as composite (i.e. not prime) the multiples of each prime, starting with the first prime number 2. The multiples of a given prime are generated as a sequence of numbers starting from that prime, with constant difference between them  that is equal to that prime. This is the sieve's ket distinction from using trial division to sequentially test each candidate number for divisibility by each prime.

> algorithm Sieve of Eratosthenes
> 
> input an integer n > 1
>
> output all prime numbers from 2 and through n
>
> let A be an array of Boolean valuesm indexed by integers 2 to n.
> 
> initially all set to true
>
> for i = 2, 3,4,... not exceeding sqrt(n) do
>
> if A[i] is true
>
> for j = i ** 2, i ** 2+1,.... not exceeding n do
>
> A[j]=false
>
> return all i such that A[i] is true
