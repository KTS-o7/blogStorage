+++
title = 'Problem 153 Find Minimum in Rotated Sorted Arrat'
date = 2024-07-29T22:30:01+05:30
draft = false
series = 'leetcode'
tags =['binary-search']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)

## Question

Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

You must write an algorithm that runs in `O(log n)` time.

## Example 1

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

## Example 2

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

## Example 3

```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times.
```

## Constraints

```markdown
- `n == nums.length`
- `1 <= n <= 5000`
- `-5000 <= nums[i] <= 5000`
- All the integers of `nums` are unique.
- `nums` is sorted and rotated between `1` and `n` times.
```

## Solution

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size()-1;
        int mid;
        while(l<r){
            mid = l+(r-l)/2;
            if(nums[mid]<nums[r])
                r=mid;
            else
                l =mid+1;
        }
        return nums[l];
    }
};
```

## Complexity Analysis

```markdown
| Algorithm     | Time Complexity | Space Complexity |
| ------------- | --------------- | ---------------- |
| Binary Search | O(log n)        | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- Since the question wants a solution in O(log n) time, we can use binary search.
- how can we use binary search in a rotated sorted array?
- We can use the property of the rotated sorted array.
- If we divide the array into two parts, one part will always be sorted.
- We can use this property to find the minimum element.
- If the mid element is less than the right element, then the right part is sorted.
- If the mid element is greater than the right element, then the left part is sorted.
- We should always move towards the unsorted part.
- Because minimum will be always next to the maximum element.
- We can use this property to find the minimum element.
```

### 2. Implementation

```markdown
- Initialize `l` to 0 and `r` to `nums.size()-1`.
- Initialize `mid` to 0.
- Run a while loop until `l` is less than `r`.
- Calculate `mid` as `l+(r-l)/2`.
- If `nums[mid]<nums[r]` then move `r` to `mid`.
- Else move `l` to `mid+1`.
- Return `nums[l]`.
```

> This is a variation of binary search. We are using the property of the rotated sorted array to find the minimum element. We are moving towards the unsorted part of the array to find the minimum element. This will give us the minimum element in O(log n) time.
