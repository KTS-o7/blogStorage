+++
title = 'Problem 200 Number of Islands'
date = 2024-04-19T13:00:30+05:30
draft = false
series = 'leetcode'
tags = ['cpp','matrix','dfs','array']
toc = false
+++

# Problem Statement

**Link** - [Problem 200](https:leetcode.com/problems/number-of-islands/description/)

## Question

Given an `m x n` 2D binary grid grid which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Note to self

- They use characters not integers for `1` and `0`.
- This is a classic dfs problem.

## Example 1

**Input:**

```text
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
```

**Output:**

```text
1
```

## Example 2

**Input:**

```text
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```

**Output:**

```text
3
```

## Constraints

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

## Solution

```cpp
class Solution {
public:
void checkIsland(int row,int col,vector<vector<char>>& grid)
{
    if(row < 0 || col < 0 || row >= grid.size() || col >= grid[0].size() || grid[row][col] != '1')
                return;
            grid[row][col] = '0';
            checkIsland(row - 1, col,grid);
            checkIsland(row + 1, col,grid);
            checkIsland(row, col - 1,grid);
            checkIsland(row, col + 1,grid);
}
    int numIslands(vector<vector<char>>& grid) {
        std::ios::sync_with_stdio(false);
        if(grid.empty() || grid[0].empty())
            return 0;

        int rows = grid.size();
        int cols = grid[0].size();
        int islands = 0;
        for(int row = 0; row < rows; row++) {
            for(int col = 0; col < cols; col++) {
                if(grid[row][col] == '1') {
                    checkIsland(row, col,grid);
                    islands++;
                }
            }
        }

        return islands;
    }
};
```

## Complexity

- `Time : O(m*n)`
- `Space : O(m*n)`

## Explaination

1. We use the classic dfs approach to solve this.
2. The logic is to check if the current cell is in bounds, and is `'1'`.
3. If it is then the cell is marked as visited and call the function recursively on all the 4 directions.
4. This recursive call checks if its adjacent cells are `'0'` or `'1'`.
5. If they all are Zeroes then the call will end and the count of islands `Islands` will be incremented.
6. If the current cell has an unvisited land neighbour then we extend our search there.
7. So whenever the `checkIsland` function is ended by the base case it means it has found a valid island.

---

> This demonstrates the use of DFS to traverse a 2D matrix and find the components or groups.
