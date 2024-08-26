+++
title = 'Problem 1509 Minimum Difference Between Largest and Smallest Value in Three Moves'
date = 2024-07-03T21:40:45+05:30
draft = false
series = 'leetcode'
tags =['sorting','greedy']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1509](https://leetcode.com/problems/minimum-difference-between-largest-and-smallest-value-in-three-moves/description/)

## Question

You are given an integer array `nums`.

In one move, you can choose one element of `nums` and change it to **any value**.

Return the minimum difference between the largest and smallest value of `nums` after performing **at most three moves**

## Example 1

```
Input: nums = [5,3,2,4]
Output: 0
Explanation: We can make at most 3 moves.
In the first move, change 2 to 3. nums becomes [5,3,3,4].
In the second move, change 4 to 3. nums becomes [5,3,3,3].
In the third move, change 5 to 3. nums becomes [3,3,3,3].
After performing 3 moves, the difference between the minimum and maximum is 3 - 3 = 0
```

## Example 2

```
Input: nums = [1,5,0,10,14]
Output: 1
Explanation: We can make at most 3 moves.
In the first move, change 5 to 0. nums becomes [1,0,0,10,14].
In the second move, change 10 to 0. nums becomes [1,0,0,0,14].
In the third move, change 14 to 1. nums becomes [1,0,0,0,1].
After performing 3 moves, the difference between the minimum and maximum is 1 - 0 = 1.
It can be shown that there is no way to make the difference 0 in 3 moves.
```

## Example 3

```
Input: nums = [6,6,0,1,1,4,6]
Output: 2
Explanation: We can make at most 3 moves.
In the first move change 0 to 6
In the second move change 1 to 6
In the third move change 1 to 6
After performing 3 moves, the difference between the minimum and maximum is 6 - 4 = 2
```

## Constraints

```
- 1 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9
```

## Solution

```cpp
class Solution {
public:
    int minDifference(vector<int>& nums) {
        if(nums.size()<=4)
            return 0;
        std::ios::sync_with_stdio(false);
        sort(nums.begin(),nums.end());
        int ans = INT_MAX;
        for(int i = 0; i <= 3; i++) {
            ans = min(ans, nums[nums.size() -1 -(3 - i)] - nums[i]);
        }
        return ans;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(nlogn)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- To get the minimum difference between the largest and smallest value
  we have the following logic.
- If the size of array is less than or equal to 4
  then we can change every element to the same value, hence difference will be 0.
- If the size of the array is greater than 4 then we need to sort the array.
- Since we have 3 moves it means we can change 3 numbers.
- These are the possible combinations we can do to find a lower difference.
  1. make the last 3 elements same as first element.(equivalent of removing last 3 elements.)
     then the difference will be nums[nums.size() - 4] - nums[0]
  2. make the first 3 elements same as last element. (equivalent of removing first 3 elements.)
     then the difference will be nums[nums.size() - 1] - nums[3]
  3. make first 2 elements and last element same.(equivalent of removing first 2 elements and last element)
     then the difference will be nums[nums.size()-2]-nums[2]
  4. make the last 2 elements and first element same.(equivalent of removing last 2 elements and first element)
     then difference will be nums[nums.size()-3]-nums[1]
- The answer will be minimum of these 4.
```

### 2. Implementation

```markdown
- If the size of `nums` is less than or equal to 4 then return 0.
- Else sort the array.
- Initialize `ans` to `INT_MAX`.
- Iterate from 0 to 3.
  - Update `ans` to minimum of `ans` and `nums[nums.size() -1 -(3 - i)] - nums[i]`.
- This will calculate the above mentioned 4 combinations and return the minimum difference.
```
