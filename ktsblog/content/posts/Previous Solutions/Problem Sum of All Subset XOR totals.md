+++
title = 'Problem 1863 Sum of All Subset XOR Totals'
date = 2024-05-20T22:32:09+05:30
draft = false
series = 'leetcode'
tags =['backtracking','bit-manipulation','recursion','cpp']
toc = false
+++

# Problem Statement

**Link** - [Problem 1863](https://leetcode.com/problems/sum-of-all-subset-xor-totals/description/)

## Question

The XOR total of an array is defined as the **bitwise** `XOR` of all its elements, or `0` if the array is empty.

For example, the XOR total of the array `[2,5,6]` is `2 XOR 5 XOR 6 = 1`.
Given an array `nums`, return the sum of all XOR totals for every subset of `nums`.

Note: Subsets with the same elements should be counted multiple times.

An array `a` is a subset of an array `b` if `a` can be obtained from `b` by deleting some (possibly zero) elements of `b`.

## Example 1

```text
Input: nums = [1,3]
Output: 6
Explanation: The 4 subsets of [1,3] are:
- The empty subset has an XOR total of 0.
- [1] has an XOR total of 1.
- [3] has an XOR total of 3.
- [1,3] has an XOR total of 1 XOR 3 = 2.
0 + 1 + 3 + 2 = 6
```

## Example 2

```text
Input: nums = [5,1,6]
Output: 28
Explanation: The 8 subsets of [5,1,6] are:
- The empty subset has an XOR total of 0.
- [5] has an XOR total of 5.
- [1] has an XOR total of 1.
- [6] has an XOR total of 6.
- [5,1] has an XOR total of 5 XOR 1 = 4.
- [5,6] has an XOR total of 5 XOR 6 = 3.
- [1,6] has an XOR total of 1 XOR 6 = 7.
- [5,1,6] has an XOR total of 5 XOR 1 XOR 6 = 2.
0 + 5 + 1 + 6 + 4 + 3 + 7 + 2 = 28
```

## Example 3

```text
Input: nums = [3,4,5,6,7,8]
Output: 480
Explanation: The sum of all XOR totals for every subset is 480.
```

## Constraints

- `1 <= nums.length <= 12`
- `1 <= nums[i] <= 20`

## Solution

```cpp
class Solution {
public:
    int subsetXORSum(vector<int>& nums) {
        std::ios::sync_with_stdio(false);
        return calculate(nums, 0, 0);
    }

    int calculate(vector<int>& nums, int level, int currentXOR) {

        if (level == nums.size())
            return currentXOR;

        int include = calculate(nums, level + 1, currentXOR ^ nums[level]);

        int exclude = calculate(nums, level + 1, currentXOR);

        return include + exclude;
    }
};
```

## Complexity Analysis

- `Time Complexity`: O(2^n), where n is the length of nums. This is because there are 2^n subsets.
- `Space Complexity`: O(n), which is the depth of the recursion tree.

## Explanation

### 1. Intuition

- The problem requires calculating the XOR total of all subsets.
- Each element can either be included or excluded in a subset.
- We need to explore both possibilities for each element using recursion.
- Recursion Tree for Example 1
  {{<mermaid>}}
  graph TD
  A[Start nums = 1, 3 currentXOR = 0] --> B[Include 1 currentXOR = 1]
  A --> C[Exclude 1 currentXOR = 0]

      B --> D[Include 3 currentXOR = 1 XOR 3 = 2]
      B --> E[Exclude 3 currentXOR = 1]

      C --> F[Include 3 currentXOR = 0 XOR 3 = 3]
      C --> G[Exclude 3 currentXOR = 0]

      D --> H[Leaf Node currentXOR = 2]
      E --> I[Leaf Node currentXOR = 1]
      F --> J[Leaf Node currentXOR = 3]
      G --> K[Leaf Node currentXOR = 0]

{{</mermaid>}}

### 2. Implementation

- The calculate function is a recursive function that explores all subsets.
- At each step, we decide whether to include or exclude the current element.
- The base case is when we've considered all elements, at which point we return the current XOR total.
- The final result is the sum of XOR totals for all subsets.

### 3. Functions

- `subsetXORSum`: Initializes the recursive process.
- `calculate`: Recursively calculates the XOR total for all subsets, either including or excluding the current element, and sums the results.

---

> This problem demonstrates the use of recursion, backtracking and bit manipulation to solve combinatorial problems.
