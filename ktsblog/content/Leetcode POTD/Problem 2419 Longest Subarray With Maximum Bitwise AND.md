+++
title = 'Problem 2419 Longest Subarray With Maximum Bitwise AND'
date = 2024-09-14T14:57:52+05:30
draft = false
series = 'leetcode'
tags =['math','bit-manipulation','array','range-query']
toc = true
math = false
+++

# Problem Statement

**Link** - [Problem 2419](https://leetcode.com/problems/longest-subarray-with-maximum-bitwise-and/description/)

## Question

You are given an integer array `nums` of size `n`.

Consider a non-empty subarray from `nums` that has the maximum possible bitwise AND.

- In other words, let `k` be the maximum value of the bitwise AND of any subarray of `nums`. Then, only subarrays with a bitwise AND equal to `k` should be considered.
  Return the length of the longest such subarray.

The bitwise AND of an array is the bitwise AND of all the numbers in it.

A subarray is a contiguous sequence of elements within an array.

## Example 1

```
Input: nums = [1,2,3,3,2,2]
Output: 2
Explanation:
The maximum possible bitwise AND of a subarray is 3.
The longest subarray with that value is [3,3], so we return 2.
```

## Example 2

```
Output: 1
Explanation:
The maximum possible bitwise AND of a subarray is 4.
The longest subarray with that value is [4], so we return 1.
```

## Constraints

- 1 <= `nums.length` <= 10<sup>5</sup>
- 0 <= `nums[i]` <= 10<sup>6</sup>

## Solution

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums) {
        int maxi = *max_element(nums.begin(),nums.end());
        int len = 1;
        int temp = 0;
        for(const auto& num:nums){
            if(num == maxi)
                temp++;
            else{
                len = max(temp,len);
                temp =0;
            }
            len = max(temp,len);
        }
        return len;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm               | Time Complexity | Space Complexity |
| ----------------------- | --------------- | ---------------- |
| Get Max Element         | O(n)            | O(1)             |
| Compute subarray length | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

Question wants us to find the length of the subarray whose bitwise AND is maximum.

- If we observe the property of BITWISE AND between two numbers, the result can be at most the minimum of the two numbers.
- So, that means for a generic subarray the maximum bitwise AND can be the minimum of the subarray.
- This means that to obtain the maximum bitwise AND, we need to find the maximum element in the array and the subarray in consideration should have all the elements as the maximum element.

Using the above observation, if we are able to find the maximum element in the array and determine the maximum length of the subarray whose elements are the maximum element, that will be the answer.

### 2. Implementation

```markdown
- Initialize `maxi` as the maximum element in the array.
- Use `*max_element` to find the maximum element in the array and store it in `maxi`.
- Initialize `len` to store the length of the subarray.
- Initialize `temp` to store the length of the current subarray.
- Iterate over all the elements in the array. For each element:
  - If the element is equal to `maxi`, increment `temp`.
  - Else, update `len` to the maximum of `temp` and `len` and reset `temp` to 0.
- Update `len` to the maximum of `temp` and `len`.
- Return `len`.
```

> This is a highly observation based problem. The key is to understand the property of bitwise AND and how it can be used to solve the problem.
