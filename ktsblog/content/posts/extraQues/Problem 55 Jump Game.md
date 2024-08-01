+++
title = 'Problem 55 Jump Game'
date = 2024-07-30T00:52:44+05:30
draft = false
series = 'leetcode'
tags =['dynamic-programming','dp','greedy']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 55](https://leetcode.com/problems/jump-game/description/)

## Question

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

## Example 1

```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1,
then 3 steps to the last index.
```

## Example 2

```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what.
Its maximum jump length is 0,
which makes it impossible to reach the last index.
```

## Constraints

```markdown
- `1 <= nums.length <= 10^4`
- `0 <= nums[i] <= 10^5`
```

## Solution

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
    std::ios::sync_with_stdio(false);
    int numsSize = nums.size();
    if (numsSize ==1)
    return true;


        int i,jump,flag=0;
        for(i=0;i<numsSize;i++)
        {
           flag = max(nums[i] + i ,flag );
           if(flag <i+1)
           break;

        }
        return  flag >= numsSize-1;

    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Greedy    | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- Seeing the constraints, we can solve this problem using a greedy approach.
- What we can do is, we can keep track of the maximum index we can reach from the current index.
- Starting from the 0th index, we can update the maximum index we can reach from the current index.
- If the maximum index we can reach from the current index is less than the next index to the current index,
  then we can break the loop.
- If the maximum index we can reach from the current index is greater than or equal to the last index, then we can return true.
- Otherwise, we can return false.
```

### 2. Implementation

```markdown
- We can initialize the variable `flag` to 0.
- `flag` will store the maximum index we can reach from the current index.
- We can iterate over the array.
- For every index, calculate `flag = max(nums[i] + i, flag)`.
- If `flag` is less than `i+1`, then we can break the loop.
- If `flag` is greater than or equal to the last index, then we can return true.
- Otherwise, we can return false.
```
