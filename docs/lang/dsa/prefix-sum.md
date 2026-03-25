A prefix sum (also called cumulative sum or running total) is a technique where we precompute the sum of all elements from the beginning of an array up to each index.

This preprocessing allows us to answer range sum queries in constant time.

Given an array nums of length n:

```bash
prefix[0] = nums[0]
prefix[1] = nums[0] + nums[1]
prefix[2] = nums[0] + nums[1] + nums[2]
...
prefix[i] = nums[0] + nums[1] + ... + nums[i]
```

In other words, prefix[i] stores the sum of all elements from index 0 to index i.

Once we have the prefix sum array, we can find the sum of any range [left, right] using a simple formula:

sum(left, right) = prefix[right] - prefix[left - 1]

```bash
example array [2, 4, 1, 3, 5]:
-----index---- 0  1  2  3  4
sum(2, 4) = prefix[4] - prefix[1] = 15 - 6 = 9
Let us verify: nums[2] + nums[3] + nums[4] = 1 + 3 + 5 = 9 ✓
```

Edge case: When left = 0, there is no prefix[left - 1]. In this case, sum(0, right) = prefix[right].


## 子数组和等于k

Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.

A subarray is a contiguous non-empty sequence of elements within an array.

Example 1:
Input: nums = [1,1,1], k = 2

Output: 2

Example 2:
Input: nums = [1,2,3], k = 3

Output: 2 

[Go代码示例](https://github.com/ashe-c0de/lang-lab/blob/main/golang/algorithm/prefix-sum.go)