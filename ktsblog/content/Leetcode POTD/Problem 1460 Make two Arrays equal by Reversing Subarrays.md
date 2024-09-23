+++
title = 'Problem 1460 Make Two Arrays Equal by Reversing Subarrays'
date = 2024-09-19T22:36:08+05:30
draft = false
series = 'leetcode'
tags =['array','counting','hash-table','sorting']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1460](https://leetcode.com/problems/make-two-arrays-equal-by-reversing-subarrays/description/)

## Question

You are given two integer arrays of equal length `target` and `arr`. In one step, you can select any non-empty subarray of `arr` and reverse it. You are allowed to make any number of steps.

Return `true` if you can make `arr` equal to `target` or `false` otherwise.

## Example 1

```
Input: target = [1,2,3,4], arr = [2,4,1,3]
Output: true
Explanation: You can follow the next steps to convert arr to target:
1- Reverse subarray [2,4,1], arr becomes [1,4,2,3]
2- Reverse subarray [4,2], arr becomes [1,2,4,3]
3- Reverse subarray [4,3], arr becomes [1,2,3,4]
There are multiple ways to convert arr to target, this is not the only way to do so.
```

## Example 2

```
Input: target = [7], arr = [7]
Output: true
Explanation: arr is equal to target without any reverses.
```

## Example 3

```
Input: target = [3,7,9], arr = [3,7,11]
Output: false
Explanation: arr does not have value 9 and it can never be converted to target.
```

## Constraints

- `target.length == arr.length`
- `1 <= target.length <= 1000`
- `1 <= target[i] <= 1000`
- `1 <= arr[i] <= 1000`

## Solution

```cpp
class Solution {
public:
    bool canBeEqual(vector<int>& target, vector<int>& arr) {
        vector<int> cnt1(1001);
        vector<int> cnt2(1001);
        for (int& v : target) {
            ++cnt1[v];
        }
        for (int& v : arr) {
            ++cnt2[v];
        }
        return cnt1 == cnt2;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Counting  | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition

We need to create subarrays of `arr` and reverse them to obtain target. But the key point is we can do this operation any number of times.

- This means we can take two element subarrays and reverse them to essentially generate the target array given that the elements are the same.
- Because of this we just need to check if the frequency of elements in both arrays is the same.
- If the frequency of elements is the same, we can generate the target array by reversing subarrays.
- If the frequency of elements is not the same, we cannot generate the target array by reversing subarrays.

### 2. Implementation

```markdown
- Initialize two vectors `cnt1` and `cnt2` of size 1001 with 0.
- Iterate over the elements of `target` and increment the count of the element in `cnt1`.
- Iterate over the elements of `arr` and increment the count of the element in `cnt2`.
- Return whether the two vectors are equal.
```
