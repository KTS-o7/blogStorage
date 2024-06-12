+++
title = 'Problem Sort Colors'
date = 2024-06-12T18:48:37+05:30
draft = false
series = 'leetcode'
tags =['sorting','two-pointer','array']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 75](https://leetcode.com/problems/sort-colors/description/)

## Question

Given an array `nums` with `n` objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

**You must solve this problem without using the library's sort function.**

## Example 1

```text
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

## Example 2

```text
Input: nums = [2,0,1]
Output: [0,1,2]
```

## Constraints

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` is either `0`, `1`, or `2`.

## Solution

```cpp
// Two loop solution
class Solution {
public:
    void sortColors(vector<int>& nums) {
       for(int i = 0;i<nums.size();i++)
       {
           for(int j = i+1;j<nums.size();j++)
           {
               if(nums[i]>nums[j])
            {
                int temp = nums[i];
                nums[i]=nums[j];
                nums[j]=temp;
            }
           }
       }
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N^2)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- Since they need it to be in-place, we can use two loops to sort the array.
- This is similar to bubble sort.
```

### 2. Implementation

```markdown
- Outer loop runs from 0 to n-1.
- Inner loop runs from i+1 to n.
- If nums[i] > nums[j], then swap the elements.
- Continue this process until the array is sorted.
- This will ensure that the array is sorted in-place.
```

### Alternate Solution

- Dutch National Flag Algorithm

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int low = 0;
        int mid = 0;
        int high = nums.size()-1;
        while(mid<=high)
        {
            if(nums[mid]==0)
            {
                swap(nums[low],nums[mid]);
                low++;
                mid++;
            }
            else if(nums[mid]==1)
            {
                mid++;
            }
            else
            {
                swap(nums[mid],nums[high]);
                high--;
            }
        }
    }
};
```

- Time Complexity : O(N)
- Space Complexity : O(1)

#### Intuition

```markdown
- We can use the Dutch National Flag Algorithm to solve this problem.
- We use three pointers to keep track of the elements.
- We use `low` to keep track of the 0's.
- We use `mid` to keep track of the 1's.
- We use `high` to keep track of the 2's.
- We traverse through the array and swap the elements accordingly.
```

#### Implementation

```markdown
- Initialize `low` to 0, `mid` to 0, and `high` to n-1.
- Traverse through the array until `mid` is less than or equal to `high`.
- If `nums[mid]` is 0, then swap `nums[low]` and `nums[mid]` and increment `low` and `mid`.
- If `nums[mid]` is 1, then increment `mid`.
- If `nums[mid]` is 2, then swap `nums[mid]` and `nums[high]` and decrement `high`.
- Continue this process until `mid` is less than or equal to `high`.
- This will ensure that the array is sorted according to the colors.
```

> Shows a similarity to count sort. We can use the same approach to solve this problem. We can count the number of 0's, 1's, and 2's and then fill the array accordingly. This will also solve the problem in O(N) time complexity.
