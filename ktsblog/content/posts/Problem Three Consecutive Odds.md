+++
title = 'Problem Three Consecutive Odds'
date = 2024-07-01T18:55:41+05:30
draft = false
series = 'leetcode'
tags =['array','parity']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1550](https://leetcode.com/problems/three-consecutive-odds/description/)

## Question

Given an integer array `arr`, return `true` if there are **three consecutive odd numbers** in the array. Otherwise, return `false`.

## Example 1

```
Input: arr = [2,6,4,1]
Output: false
Explanation: There are no three consecutive odds.
```

## Example 2

```
Input: arr = [1,2,34,3,4,5,7,23,12]
Output: true
Explanation: [5,7,23] are three consecutive odds.
```

## Constraints

```markdown
- `1 <= arr.length <= 1000`
- `1 <= arr[i] <= 1000`
```

## Solution

```cpp
class Solution {
public:
    bool threeConsecutiveOdds(vector<int>& arr) {
        if(arr.size()==1||arr.size()==2)
        return false;
        for(int i=0;i<arr.size()-2;i++)
        {
            if(arr[i]%2!=0)
            {
                if(arr[i+1]%2 !=0 && arr[i+2]%2 !=0)
                {
                   return true;
                }
            }

        }
      return false;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(n)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- We just need to check if there are three consecutive odd numbers in the array.
- If we find such a sequence, we return true.
- Otherwise, we return false.
- If we find the current number to be odd check if next two numbers are also odd.
```

### 2. Implementation

```markdown
- If the size of the array is less than 3, return false.
- Iterate from `0` to `n-2`.
- Check if `arr[i]` is odd.
- If it is odd, check if `arr[i+1]` and `arr[i+2]` are also odd.
- If they are, return true.
- If no such sequence is found, return false.
```
