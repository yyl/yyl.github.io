---
layout: post
title:  "Choose a subset"
date:   2014-12-11 22:00:00
categories: leetcode
---

A lot of problems on leetcode ask people to find a subset from a set of values to meet certain requirements. 

1.[Best Time to Buy and Sell Stock I](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock/)

This problem asks for a buy and sell date that maximize the profit, given a list of prices. To solve this problem, we keep track of the current lowest and highest price, which latter has to be after the former. Then we do a list traversal, and do:

1. if current price is lower than the lowest price, update the lowest index
2. if current price is larger than highest price, update the highest index

The [follow-up](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) allows multiple buy and sell (although one could only buy or sell on one day). To solve the problem, greedy is good. We traverse the list of prices, if current price is higher than the previous one, we buy at previous price and sell it with current price. Such method will guarantee us the maximum profit.

2.[Maximum Subarray](https://oj.leetcode.com/problems/maximum-subarray/)

The problem is to find the contiguous subarray within an array (containing at least one number) which has the largest sum.

In this problem, we keep track of global maximum sum, and current sum. We scan through the array, do:

1. add current value and update current sum
2. if current sum is negative, reset it to 0
3. if current sum is bigger than maximum sum, update the maximum sum

This algorithm runs \\(O(n)\\) in place. The intuition is similar to previous problem, but instead of two values, we need a contiguous set of values. Therefore, we need to remove values sum up to negative, as they do not help to maximize the sum.

However, this algorithm does not work when array only contain negative value. Therefore, we need to add a check before the algorithm.

3.[Container With Most Water](https://oj.leetcode.com/problems/container-with-most-water/)

The problem gives a list of integers, each of which represents a vertical line on the position given by the index. It asks for two indices, which combine with their values to be a container that containing the most water.

To contain most water, we need two values such that the value given by the equation below is maximized.

$$min(A[i], A[j]) \times |j-i|$$

A brute force algorithm will be running through all values and for each one find the one value in later position that maximize the equation. This needs \\(O(n^2)\\).

Here we try to be greedy. We start from the beginning, and calculate the container size of say the first and last values. Then we move the two pointers, always try to increase the smaller one, and compute the container size, so that we will not miss a size that is larger than our current one. This needs \\(O(n)\\) time.

4.[Longest Consecutive Sequence](https://oj.leetcode.com/problems/longest-consecutive-sequence/)

This problem asks to pick a subset of values from given array so that they could form the longest consecutive sequence; return the number of values in the sequence.

To solve this problem, one needs to realize the fact that one value of the array could only be in one consecutive sequence. Therefore, if we could keep track of values that have already been in a sequence, we could find the longest one.

To track that, we create a hash table to store key value pair. This is also used to find consecutive sequence. We could first do a scan to store every value in the hash table, marked as unvisited.

With the hash table, we scan through the array, for each value we do:

- check if it has already been in a sequence by looking up in the hash table
	- if it is, move on
	- otherwise, decrease and increase its value to formulate a consecutive sequence until value does not exist in the hash table
	- get current length and update the maximum length if applicable
	- mark all used value visited in the hash table

This algorithm gives us a \\(O(n)\\) time complexity with \\(O(n)\\) space complexity.

5.[Longest Common Prefix](https://oj.leetcode.com/problems/longest-common-prefix/)

This problem gives a set of strings and ask for longest common prefix of them.

Since longest common prefix has to be in all of the strings, we just pick anyone, and start with its first character, check rest of strings, and stop at the point where strings do not match. This algorithm needs \\(O(n \times k)\\) time where `k` is the length of the shortest string.

This problem should have a better solution using say [suffix tree](http://en.wikipedia.org/wiki/Suffix_tree), which is also a classic algorithm problem. But for solving leetcode, we rarely need that kind of complex algorithm.

TBD

### conclusion

So far, we could see two keys appear most often in this kind of problems. First is greedy, we either always look for profit, or we always maintain the highest/longest value. Second, 2 or more pointers are used to keep track of start/end, or multiple values so that to ensure a certain calculation is maximized/minimized.
