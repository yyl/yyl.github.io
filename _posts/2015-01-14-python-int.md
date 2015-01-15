---
layout: post
title:  "Integer in Python"
date:   2015-01-14 21:30:00
categories: python
---

In this post I hope to summarize what consists of the data type `int` in Python.

### 2 types of integer

There are two types of integer variable, plain and long. The difference in representation is value in long format ends with `L`. `1` is plain integer, and `1L` is a long integer. 

The reason for the existence of two integer types is long integer could hold up larger values. The plain integer has a size limit based on the machine it runs. For my 64-bit machine, the maximum number a python plain integer could hold is \\(2^{63}\\), and the maximum negative one is \\(-2^{63}-1\\). The maximum number is denoted as `sys.maxint`. However, for a long integer, there is no preset limit for the value it could hold. How large it could hold up depends on the memory the machine could offer. 

Python would automatically convert the plain integer variable into a long integer variable if the value it holds somehow increases beyond the maximum value it could hold. Unless you explicitly tell the interpreter the integer is a long format by appending `L` at the end of the integer value, the integer variables created are plain integers by default.

### Integer overflow

The existence of long integer format in Python prevents the integer overflow error that happens in C or Java happens in Python. We review the classic integer overflow problem here.

In a computer, a number is represented as binary number. Normally, a signed binary representation system uses one bit to indicate the sign of the number: `1` means negative, and `0` means positive. Therefore, a k-bit signed number system only have \\((k-1)\\) bits to hold value, which means the largest positive number a k-bit signed binary system could hold is \\(2^{k-1}\\). However, the largest negative number it could hold is \\(-2^k\\), which is represented as \\((10000...00)_2\\), where we have \\(k-1\\) zeros.

Imagine in a 8-bit signed number system. \\(2\\) would be represented as \\((00000010)_2\\). Note the left-most bit is used as the sign, which in this case is zero, meaning it is positive. \\(-2\\) would be \\((10000010)_2\\). The only difference is the sign.  Also, \\((00000000)\_2\\) indicates zero, and \\((10000000)\_2\\) indicates \\(-2^8\\).

When it overflows, what would the value be? We know the binary representation of \\(2^7\\) is \\(1111111\\). Like we said, there is one more bit to represent the sign, which in our case is zero as the value is positive. Therefore, the number in binary representation is \\((01111111)\_2\\). When we add 1, it becomes \\((10000000)\_2\\), which by the definition of the system, represents \\(-256\\).

Although apparently people using Python [miss integer overflow](http://stackoverflow.com/questions/7770949/simulating-integer-overflow-in-python).
