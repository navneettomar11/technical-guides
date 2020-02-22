# Bit Shifting
A **bit shift** moves each digit in a number's binary representation left or right. There are three main types of shifts:

## Left Shifts
When shifting left, the most-significant bit is lost, and a 0 bit is inserted on the other end.
> 0010 << 1 --> 0100
> 0010 << 2 --> 1000
A single left shift multiplies a binary number by 2:
> 0010 << 1 --> 0100
> 0010 --> 2
> 0100 --> 4

## Logical Right Shifts
When shifting right with a **logical right shift**, the least-significant its is lost and a 0 is inserted on the other end.
> 1011 >>> 1 --> 0101
> 1011 >>> 3 --> 0001
For postive numbers, a single logical shift divides a number by 2, throwing out any remainders.

> 0101 >>> 1 --> 0010
> 
> 0101 is 5
> 0010 is 2

## Arithmetic Right Shifts
When shifting right with an **arithmetic right shift**, the least-significant bits is lost and the most significant bit is copied.

Language handle arithmetic and logical right shifting in different ways. Java provides two right shift operators: >> does an arithmetic right shift and >>> does a logical right shift.

> 1011 >> 1 --> 1101
> 1011 >> 3 --> 1111
>
> 0011 >> 1 --> 0001
> 0011 >> 2 --> 0000

The first two numbers had a 1 as the most significant bit, so more 1's were inserted during the shift. The last tw0 numbers had a 0 as the most significant bit, so the shift inserted more 0's.

If a number is encoded using two's complemenet, then an arithmetic right shift preserves the number's sign, while a logical right shift makes the number postive.

> //Airthmetic shift
> 1011 >> 1 --> 1101
> 1011 is -5
> 1101 is -3
>
> //Logical Shift
> 1111 >>> 1 --> 0111
> 1111 is -1
> 0111 is 7

## Check if a number is divisble by 17 using bitwise operators
Given a number n check if it is divisible by 17 using bitwise operators
Examples:
> Input: n = 34
> Output: 34 is divisible by 17

To do division using Bitwise operators, we must rewrite the expression in power of 2
> n/17 = (16 * n)/ (17*16)
>      =  (17 -1) * n / (17*16)
>      =  n/16 - (n/17*16)
We can rewrite n/16 as floot(n/16) + (n%16)/16 using general rule of divison
> n/17 = floor(n/16) + (n%16)/16 - (floor(n/16) + (n%16)/16)/17
> = floor(n/16) - (floor(n/16) - n%16)/17

Bitwise : (n >> 4) - (n & 15)


