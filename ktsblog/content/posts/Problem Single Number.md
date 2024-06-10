+++
title = 'Problem Single Number'
date = 2024-06-10T21:33:50+05:30
draft = false
series = 'leetcode'
tags =['bit-manipulation','vectors','bitwise-xor']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 136](https://leetcode.com/problems/single-number/description/)

## Question

Given a **non-empty** array of integers `nums`, every element appears twice except for one. Find that single one.

**You must implement a solution with a linear runtime complexity and use only constant extra space.**

### Note

- We need O(1) space and O(n) time complexity.
- That means we cant use any other space to store frequency of elements and cant sort the array and check for the element which is not repeated.
- This hints that we need to manipulate the given array and find the single number.

## Example 1

```text
Input: nums = [2,2,1]
Output: 1
```

## Example 2

```text
Input: nums = [4,1,2,1,2]
Output: 4
```

## Example 3

```text
Input: nums = [1]
Output: 1
```

## Constraints

- `1 <= nums.length <= 3 * 10^4`
- `-3 * 10^4 <= nums[i] <= 3 * 10^4`
- Each element in the array appears twice except for one element which appears only once.

## Solution

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        int ans = nums[0];
        for(int i= 1; i<nums.size(); i++)
            ans = ans^nums[i];
        return ans;

    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- We know that a number XOR with itself is 0.
- So, if we XOR all the elements in the array,
  the elements which are repeated will cancel each other.
- We will be left with the single number.
```

### 2. Implementation

```markdown
- Initialize a variable `ans` with the first element of the array.
- Iterate over the array from the second element.
- XOR the `ans` with the current element.
- Return the `ans`.
```

> This solution shows the power of XOR operation. XOR operation is commutative and associative. So, we can use it to find the single number in the array.
