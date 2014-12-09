---
layout: post
title:  "Hash table"
date:   2014-12-09 12:00:00
categories: leetcode
---

## leetcode problems

1.[Single Number](https://oj.leetcode.com/problems/single-number/)

The problem asks to find the number that appears only once in the array while rest of them all appear twice. This problem has a better solution without extra space, but that is more _brain teaser_ type. A hash table solution is more intuitive.

The key idea here is to create a hash table, scan through the array and store every key in the table. Then we just traverse the table and find the key value pair where there are only one value. The time complexity and space complexity will both be \\(O(n)\\).

2.[Anagrams](https://oj.leetcode.com/problems/anagrams/)

[Anagram](http://en.wikipedia.org/wiki/Anagram) is a concept where two words consist of exactly same characters but in different orders. This problems gives a list of strings, and ask to return groups of strings that are anagrams. For example, input is `["str", "srt", "tr", "rt", "s"]`, then the output should be `[["str","srt"],["tr","rt"]]`. Ideally the anagram should also be a proper word, but this problem does not require that.

The intuition to use hash table here is to store each string with its all characters sorted in an array as the key. Therefore, all strings using same words will be stored in the same position, i.e. group into anagrams.

3.[Two Sum](https://oj.leetcode.com/problems/two-sum/)

This problem gives a list of integers and a target value, and asks to return a pair of integers from the list whose sum is the target value. It assumes for each target value there will be only one pair of integers to match it.

The naive approach would be for each integer in the list traverse the rest of the list to see if the two matches the target, which gives a \\(O(n^2)\\) time complexity.

To solve this problem with hash table, there are 2 steps. First, one traverse the list, for each integer, store key value pair `(target-A[i], A[i])` in the table; second, traverse the list again, for each integer, look up the table for `table[A[i]]`. The one query that returns value instead of NULL would be what we want. This reduces time complexity into \\(O(n)\\)

This strategy could be extended to different variations of the problem, such as [4Sum](https://oj.leetcode.com/problems/4sum/), etc. One could always construct the hash table of `(target-a, a)` in first scan, and find answer in the second scan. Comparing with brute force, this strategy essentially reduces \\(O(n^k)\\) to \\(O(n^{k/2})\\). Therefore, it works best for even sum, like 2sum and 4sum we mentioned.

Alternative solution for "kSum" problem is to sort first, and have 2 pointers to scan rest of the list for each integer or sets of integers (\\(k>2)\\), which gives a \\(O(n^{k-2})\\) time complexity. This method has advantage that it does not require extra space like hash table does.

4.[Binary Tree Inorder Traversal ](https://oj.leetcode.com/problems/binary-tree-inorder-traversal/)

Another popular usage of hash table is to store visited nodes in a traversal/scan problem like the one mentioned here. As in-order traversal requires to examine the left child of the node first, and then the node itself, it is possible to go into infinite loop where you examine the left child of the node again after the node itself. Without this step the traversal could not complete.

In such situation, it is good to have a hash table to store visited nodes, and check it each time we decide to examine the new node or not. As most traversal problems require at least \\(O(n)\\) (you have to look at every item at least once), hash table does not introduce extra time complexity.

Similar problems are [Valid Sudoku](https://oj.leetcode.com/problems/valid-sudoku/) and [Longest Substring Without Repeating Characters](https://oj.leetcode.com/problems/longest-substring-without-repeating-characters/).

### Reference

\[1\] [wiki](http://en.wikipedia.org/wiki/Hash_table)