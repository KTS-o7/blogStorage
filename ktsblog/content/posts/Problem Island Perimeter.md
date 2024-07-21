+++
title = 'Problem Island Perimeter'
date = 2024-04-18T20:48:14+05:30
draft = false
series = 'leetcode'
tags = ['cpp','matrix','array']
toc = false
+++

# Problem Statement

**Link** - [Problem 463](https:leetcode.com/problems/island-perimeter/description/)

## Question

You are given row x col `grid` representing a map where `grid[i][j] = 1` represents land and `grid[i][j] = 0` represents water.

Grid cells are **connected horizontally/vertically (not diagonally)**. The `grid` is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

## Example 1

```text
Input: grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
Output: 16
```

## Example 2

```text
Input: grid = [[1]]
Output: 4
```

## Example 3

```text
Input: grid = [[1,0]]
Output: 4
```

## Constraints

- row == `grid.length`
- col == `grid[i].length`
- `1` <= row, col <= `100`
- `grid[i][j]` is `0` or `1`.
- There is exactly **one** island in grid.

## Solution

```cpp

class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {

        int row = grid.size(), col = grid[0].size();
        int peri = 0;
        for(int i=0;i<row;i++)
        {
            for(int j=0;j<col;j++)
            {
                if(grid[i][j]&1)
                {
                    peri+=4;
                    if(i>0 && (grid[i-1][j] & 1))
                        peri-=2;
                    if(j>0 && (grid[i][j-1] & 1))
                        peri-=2;
                }
            }
        }
        return peri;
    }
};
```

## Complexity

- `Time : O(N*M)`
- `Space : O(1)`

## Explanation

1. We iterate over the grid.
2. If the cell is land then add 4 to the perimeter.
3. Then we check the left and top of the cell.
4. If they are land then we subtract 2 from the perimeter each.
5. This is because when two lands are adjacent then they share a side and hence the perimeter is reduced by 2.
6. Finally return the perimeter.

---

> This problem demonstrates the traversal of a matrix and checking the adjacent cells.
