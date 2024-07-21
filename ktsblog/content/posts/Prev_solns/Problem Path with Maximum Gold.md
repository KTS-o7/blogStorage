+++
title = 'Problem 1219 Path With Maximum Gold'
date = 2024-05-14T22:27:55+05:30
draft = false
series = 'leetcode'
tags =['matrix','dfs','backtracking','cpp']
toc = false
+++

# Problem Statement

**Link** - [Problem 1219](https://leetcode.com/problems/path-with-maximum-gold/description)

## Question

In a gold mine `grid` of size `m x n`, each cell in this mine has an integer representing the amount of gold in that cell, `0` if it is empty.

Return the maximum amount of gold you can collect under the conditions:

- Every time you are located in a cell you will collect all the gold in that cell.
- From your position, you can walk one step to the left, right, up, or down.
- You can't visit the same cell more than once.
- Never visit a cell with `0` gold.
- You can start and stop collecting gold from any position in the grid that has some gold.

## Example 1

```text
Input: grid = [[0,6,0],[5,8,7],[0,9,0]]
Output: 24
Explanation:
[[0,6,0],
 [5,8,7],
 [0,9,0]]
Path to get the maximum gold, 9 -> 8 -> 7.
```

## Example 2

```text
Input: grid = [[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]
Output: 28
Explanation:
[[1,0,7],
 [2,0,6],
 [3,4,5],
 [0,3,0],
 [9,0,20]]
Path to get the maximum gold, 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7.
```

## Constraints

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 15`
- `0 <= grid[i][j] <= 100`
- There are at most **25** cells containing gold.

## Solution

```cpp
class Solution {
    const vector<pair<int,int>>direc = {{1,0},{-1,0},{0,-1},{0,1}};

    int checkIfAllNonZeros(vector<vector<int>>& grid){
        int count = 0;
        for(int i=0; i<grid.size(); i++){
            for(int j=0; j<grid[0].size(); j++){
                if(grid[i][j] != 0)
                    count += grid[i][j];
                else
                    return 0;
            }
        }

        return count;
    }

    int dfs(vector<vector<int>>& grid, int x, int y, const int& row, const int& col)
    {
        if(x<0 || y<0 || x>=row || y>= col || grid[x][y] == 0)
            return 0;

        int val = grid[x][y];
        grid[x][y] = 0;
        int localMax = val;
        for(const pair<int,int>& it:direc)
        {
            localMax = max(localMax, val + dfs(grid,x + it.first,y + it.second,row,col));
        }

        grid[x][y] = val;

        return localMax;
    }

public:
    int getMaximumGold(vector<vector<int>>& grid) {

        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        int row = grid.size(), col = grid[0].size();
        int maxVal = 0;

        int count = checkIfAllNonZeros(grid);
        if(count) {
            return count;
        }


        for(int i = 0; i<row; i++)
        {
            for(int j = 0; j<col; j++)
            {
                if(grid[i][j] != 0)
                    maxVal = max(maxVal, dfs(grid,i,j,row,col));
            }
        }

        return maxVal;

    }
};
```

## Complexity Analysis

- `Time Complexity`: O(4^{mn}).
- `Space Complexity`: O(mn).

## Explanation

### 1. Intuition

- We can start from any cell in the grid and move in any direction (up, down, left, right) to collect gold.
- We can't visit the same cell more than once.
- We can't visit a cell with 0 gold.
- We need to find the maximum amount of gold we can collect.
- This constraints suggest that we need to explore every possible path to find the maximum gold.
- Hence we need DFS to explore every possible path.
- But we need to backtrack so that a path once explored does not stop from exploring other paths.

### 2. Implementation

- The `dfs` function is used to explore every possible path.
- The `checkIfAllNonZeros` function is used to check if all the cells have non-zero gold.
- The edge case of all cells having non-zero gold is handled separately by the `checkIfAllNonZeros` function.
- The `direc` vector is used to move in all four directions.

### 3. Functions

- `dfs` function is used to explore every possible path.
- The input to the `dfs` function is the grid, the current cell's row and column, and the total number of rows and columns.
- If the current cell is out of bounds or has 0 gold, we return 0.
- Else we store the current cell's gold in a variable `val`.
- Then the current cell's gold is set to 0.
- We store the current cell's gold in a variable `localMax`.
- Recursively we move in all four directions and store the maximum gold in the `localMax` variable.
- If all 4 sides are 0 then the `localMax` will be equal to the current cell's gold.
- We restore the current cell's gold to the original value.
- This is to let other paths explore the current cell.
- We return the `localMax` variable.

- `getMaximumGold` function is used to find the maximum gold.
- The input to the `getMaximumGold` function is the grid.
- We initialize the `row` and `col` variables to store the total number of rows and columns.
- We initialize the `maxVal` variable to store the maximum gold.
- We check if all the cells have non-zero gold by calling the `checkIfAllNonZeros` function.
- If all the cells have non-zero gold then we return the sum of all the cells.
- Else we iterate over all the cells and call the `dfs` function to find the maximum gold.
- We return the `maxVal` variable.

---

> This problem enables us to apply DFS and backtracking to a 2 dimensional matrix.
