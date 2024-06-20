+++
title = 'Problem Magnetic Force Between Two Balls'
date = 2024-06-20T21:42:43+05:30
draft = false
series = 'leetcode'
tags =['binary-search','array','binary-search-on-answer']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1552](https://leetcode.com/problems/magnetic-force-between-two-balls/description/)

## Question

In the universe Earth C-137, Rick discovered a special form of magnetic force between two balls if they are put in his new invented basket. Rick has n empty baskets, the `ith` basket is at `position[i]`, Morty has `m` balls and needs to distribute the balls into the baskets such that the **minimum magnetic force between any two balls is maximum**.

Rick stated that magnetic force between two different balls at positions `x` and `y` is `|x - y|`.

Given the integer array `position` and the integer `m`. Return the required force.

#### Note

- This problem explaination is absolute trash. Lets De-Leetcodify it.
- Lets say we have `n` baskets and `m` balls. We need to distribute the `m` balls in the `n` baskets such that the minimum distance between any two balls is maximum.
- We need to find the maximum distance between any two balls such that we can distribute the balls in the baskets such that the minimum distance between any two balls is maximum.

## Example 1

```text
Input: position = [1,2,3,4,7], m = 3
Output: 3
Explanation: Distributing the 3 balls into baskets 1, 4 and 7 will make the magnetic force between ball pairs [3, 3, 6].
The minimum magnetic force is 3. We cannot achieve a larger minimum magnetic force than 3.
```

## Example 2

```text
Input: position = [5,4,3,2,1,1000000000], m = 2
Output: 999999999
Explanation: We can use baskets 1 and 1000000000
```

## Constraints

```markdown
- `n == position.length`
- `2 <= n <= 10^5`
- `1 <= position[i] <= 10^9`
- All integers in `position` are `distinct`.
- `2 <= m <= position.length`
```

## Solution

```cpp
class Solution {
public:
    int maxDistance(vector<int>& position, int m) {
        sort(position.begin(), position.end());
        int l = 1, r = position[position.size()-1];
        int ans = -1;
        while(l <= r) {
            int mid = l + (r - l) / 2;
            int lastPosition = position[0], balls = 1;
            for(int i = 1; i < position.size(); i++)
            {
                if(position[i] - lastPosition >= mid)
                {
                    lastPosition = position[i];
                    balls++;
                }
            }
            if(balls >= m)
            {
                ans = mid;
                l = mid + 1;
            }
            else
            {
                r = mid - 1;
            }
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
- Lets find out a strategy to accomplish this question.
- First find the maximum possible distance between any two balls.
- Then first place the first ball at the first position.
- Then try to place the next ball at the maximum possible distance from the first ball.
- If we can place all the balls at the maximum possible distance then we can say that the maximum possible distance is the answer.
- If we cannot place all the balls at the maximum possible distance then we need to reduce the maximum possible distance and try again.
```

### 2. Implementation

```markdown
- Sort the `position` array in increasing order.
- Set the `l` to 1 and `r` to the last element of the `position` array.
- Set the `ans` to -1.
- Run a binary search loop until `l` is less than or equal to `r`.
- Calculate the `mid` as `l + (r - l) / 2`.
- Set the `lastPosition` to the first element of the `position` array and `balls` to 1.
- The variable `lastPosition` will store the last position where we placed the ball.
- `balls` is set to `1` because we have already placed the first ball.
- Run a loop from `1` to `position.size()`.
- If the difference between the current position and the `lastPosition` is greater than or equal to `mid` then we can place the ball at the current position.
- This will tell us in which position we can place the ball.
- Update the value of `lastPosition` to the current position and increment the value of `balls`.
- Once the loop is over check if the value of `balls` is greater than or equal to `m`.
- If yes then set the `ans` to `mid` and set `l` to `mid + 1`.
- This will check if we can increase the distance between the balls.
- If no then set `r` to `mid - 1`.
- This will check if we can decrease the distance between the balls.
- Finally return the value of `ans`.
```

> This problem is similar to yesterday's problem. We are trying to find the maximum distance between any two balls such that we can place the balls in the baskets such that the minimum distance between any two balls is maximum. We are using binary search to find the maximum distance between any two balls. This is a very good example of binary search on answer.
