+++
title = 'Problem 62 Unique Paths'
date = 2024-07-29T23:17:43+05:30
draft = false
series = 'leetcode'
tags =['dynamic-programming','dp','combinatorics']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 62](https://leetcode.com/problems/unique-paths/description/)

## Question

There is a robot on an `m x n` grid. The robot is initially located at the top-left corner (i.e., `grid[0][0]`). The robot tries to move to the bottom-right corner (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to `2 * 10^9`.

## Example 1

```
Input: m = 3, n = 7
Output: 28
```

## Example 2

```
Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

## Constraints

```markdown
- `1 <= m, n <= 100`
```

## Solution

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 || j == 0) {

                    dp[i][j] = 1;
                } else {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }

        return dp[m - 1][n - 1];
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| DP        | O(mn)           | O(mn)            |
```

## Explanation

### 1. Intuition

```markdown
- We can reach every box in the top row in only one way
  i.e only from left side.
- Similarly, we can reach every box in the leftmost column in only one way
  i.e only from the top side.
- For every other box, we can reach it from the top or left side.
- So, the number of ways to reach a box is the sum of the number of ways to reach the box above it and the number of ways to reach the box to the left of it.
- this is a classic DP problem.
- We can create a 2D array to store the number of ways to reach every box.
- We can fill the top row and leftmost column with 1.
- For every other box, we can fill it with the sum of the number of ways to reach the box above it and the number of ways to reach the box to the left of it.
- The number of ways to reach the bottom-right corner will be stored in the last box of the 2D array.
```

### 2. Implementation

```markdown
- Initialize a 2D array `dp` of size `m` x `n` with all elements as 0.
- Fill the top row and leftmost column with 1.
- For every pair `(i,j)` where `i` is not 0 and `j` is not 0,
  `dp[i][j] = dp[i-1][j] + dp[i][j-1]`
- Return `dp[m-1][n-1]`.
```
