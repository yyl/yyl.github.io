---
layout: post
title:  "Binary search summary"
date:   2014-12-08 17:00:00
categories: leetcode
---

Time complexity: \\(O(n)\\)

Space complexity: \\(O(1)\\)

The standard description of binary search would be, given a certain value and a sorted array, find its position in the array. To achieve fast search, it compares the 1st element, last element, and the median element every time, to chop off half of the current array.

Note that binary search assumes random access to the sequence. In the standard binary search problem, _criterion_ could be simple comparison, and _search space_ would be the current array.

To go beyond a regular array, binary search applies a set of any integers, with the criterion now being a function \\(f(x)\\) where we compare its value to the target value, and search space being a interval in the function domain.

## leetcode problems

1.[Search Insert Position]()

Standard and original binary search problem.

2.[Search in Rotated Sorted Array](https://oj.leetcode.com/problems/search-in-rotated-sorted-array/)

Instead of sorted array, now it is rotated once, e.g. `4 5 6 7 0 1 2`.

This problem is similar to original binary search problem, the only difference is now we more conditions when comparing median element with the head and tail element:

`array[start] < array[mid] < array[end]`: array does not do a rotation, the problem turns into the original binary search.

`array[start] > array[mid]`: rotation happens in the first half. Then we compare `target` with elements at `start`, `mid` and `end` to decide which half to abandon.

`array[mid] > array[start] and array[start] > array[end]`: rotation happens in the second half. Again, we compare all three elements with `target` to decide which half to abandon.

[Search in Rotated Sorted Array II](https://oj.leetcode.com/problems/search-in-rotated-sorted-array-ii/) is the followup: what if duplicates are allowed in the array. Duplicates render comparison of head, tail and median elements useless because they could all be equal, and you will not know rotation happens in which half. In such case, the solution is to move forward the head element whenever they are equal, until we hit something different.

3.[Find Minimum in Rotated Sorted Array](https://oj.leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

This is similar to the second problem. We choose half of current array based on not only the comparison of head, tail and median elements, but also which half does the rotation happen.

4.[Sqrt(x)](https://oj.leetcode.com/problems/sqrtx/)

Implement `int sqrt(int x)`.

The trick here is we only need to return the closest integer, instead of the exact square root. To apply binary search on this problem, we model the _criterion_ to be \\(f(x) = x^2\\), and the _search space_ being from 1 to \\(x/2\\), as the square root cannot possibly exceed the half of the original value.

5.[Pow(x)](https://oj.leetcode.com/problems/powx-n/) 

This problem follows the same logic, in which you have to implement power function \\(f(x,n) = x^n\\). The observation here is:

$$x^n = x^{n/2} \times x^{n/2}, n\%2=0$$
$$x^n = x^{n/2} \times x^{n/2} \times x, n\%2=1$$

So that for every `n`, we look at the power result of `n/2`, which renders into a recursion problem. 

6.[Search for a Range](https://oj.leetcode.com/problems/search-for-a-range/)

The problem is to find the range of given `target` in an array. In this case the array clearly has duplicates. For example, given `8` and `[5, 7, 7, 8, 8, 10]`, return `[3,4]`.

Applying standard binary search on this problem will return just one index of `target`, without knowing the range. Therefore we need to modify the algorithm.

Instead of searching the exact value of `target`, we search for `target-0.5` and `target+0.5`. The trick here is the first search will return us the last index of the largest element that smaller than `target`, while second will return us the first index of the smallest element that larger than `target`. Then we could get the range of `target` easily.

Basically, we need to compare median element now with `target-0.5` and `target+0.5`, respectively. Also, we need to return proper index every time of the search.


### Reference

\[1\] [algorithm tutorials](http://community.topcoder.com/tc?module=Static&d1=tutorials&d2=binarySearch)