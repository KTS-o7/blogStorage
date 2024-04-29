+++
title = 'Problem Minimum Falling Path Sum II'
date = 2024-04-26T12:15:26+05:30
draft = false
toc = false
tags = ['leetcode', 'dp', 'matrix'] 
series = ""
+++

# Problem Statement

**Link**: [Problem Minimum Falling Path Sum II](https://leetcode.com/problems/minimum-falling-path-sum-ii/description/)

## Question

Given an `n x n` **integer** matrix `grid`, return the minimum sum of a **falling path with non-zero shifts**.

A **falling path with non-zero shifts** is a choice of exactly one element from each row of `grid` such that no two elements chosen in adjacent rows are in the same column.

## Example 1

|     |     |     |
| --- | --- | --- |
| 1   | 2   | 3   |
| 4   | 5   | 6   |
| 7   | 8   | 9   |

```text
Input: grid = [[1,2,3],[4,5,6],[7,8,9]]
Output: 13
Explanation:
The possible falling paths are:
[1,5,9], [1,5,7], [1,6,7], [1,6,8],
[2,4,8], [2,4,9], [2,6,7], [2,6,8],
[3,4,8], [3,4,9], [3,5,7], [3,5,9]
The falling path with the smallest sum is [1,5,7], so the answer is 13.
```

## Example 2

|     |
| --- |
| 7   |

```text
Input: grid = [[7]]
Output: 7
```

## Constraints

- `n == grid.length == grid[i].length`
- `1 <= n <= 200`
- `-99 <= grid[i][j] <= 99`

## Solution

```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& grid) {
        std::ios::sync_with_stdio(false);
        vector<vector<int>>dp(grid.size(),vector<int>(grid[0].size(),-1));
        int rows = grid.size(), cols = grid[0].size();
        int result = INT_MAX,temp;

        for(int j =0;j<cols;j++)
            dp[0][j] = grid[0][j];

        for(int i = 1;i<rows;i++)
        {
            for(int j = 0;j<cols;j++)
            {
                temp = INT_MAX;

                for(int k = 0;k<cols;k++)
                {
                    if(k!=j)
                    {
                        temp = min(temp, grid[i][j]+dp[i-1][k]);
                    }
                }
                dp[i][j] = temp;
            }
        }

        for(int j = 0;j<cols;j++)
            result = min(result,dp[rows-1][j]);

        return result;
    }
};
```

## Complexity

- `Time : O(Rows * Cols^2)`
- `Space : O(Rows * Cols)`

## Explaination

- We will use a 2D `dp` array to store the minimum sum of the falling path ending at the cell `dp[i][j]`.
- The minimum falling path sum of first row will be same as first row of `grid`, hence we initialize `dp[0][j] = grid[0][j]`.
- For each cell `dp[i][j]` we will iterate over the previous row and find the minimum sum of the falling path ending at the cell `dp[i][j]`.
- We will find the minimmum sum of falling path which is not from the same column, hence we use `if (k != j)` condition.
- The answer will be the minimum value stored at the final row of `dp` matrix.

---

> This shows the usage of iterative DP to solve a grid problem.

---

> We can also use `Dijkstras Algorithm` to solve the same problem.

---
