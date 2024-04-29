+++
title = 'Problem Find All Groups of Farmland'
date = 2024-04-20T10:52:46+05:30
draft = false
series = 'Leetcode'
tags = ['leetcode', 'cpp', 'matrix', 'dfs']
toc = false
+++

# Problem Statement

**Link** - [Problem 1992](https://leetcode.com/problems/find-all-groups-of-farmland/description/)

## Question

You are given a 0-indexed `m x n` binary matrix `land` where a `0` represents a hectare of forested `land `and a `1` represents a hectare of farmland.

To keep the `land `organized, there are designated rectangular areas of hectares that consist entirely of farmland. These rectangular areas are called groups. No two groups are adjacent, meaning farmland in one group is not four-directionally adjacent to another farmland in a different group.

`land `can be represented by a coordinate system where the top left corner of `land `is `(0, 0)` and the bottom right corner of `land `is `(m-1, n-1)`. Find the coordinates of the top left and bottom right corner of each group of farmland. A group of farmland with a top left corner at `(r1, c1)` and a bottom right corner at `(r2, c2)` is represented by the 4-length array `[r1, c1, r2, c2]`.

Return a 2D array containing the 4-length arrays described above for each **group** of farm`land `in land. If there are no groups of farmland, return an empty array. You may return the answer in **any order**.

## Example 1

```text
Input: land = [[1,0,0],[0,1,1],[0,1,1]]
Output: [[0,0,0,0],[1,1,2,2]]
Explanation:
The first group has a top left corner at land[0][0] and a bottom right corner at land[0][0].
The second group has a top left corner at land[1][1] and a bottom right corner at land[2][2].
```

## Example 2

```text
Input: land = [[1,1],[1,1]]
Output: [[0,0,1,1]]
Explanation:
The first group has a top left corner at land[0][0] and a bottom right corner at land[1][1].
```

## Example 3

```text
Input: land = [[0]]
Output: []
Explanation:
There are no groups of farmland.
```

## Constraints

- `m == land.length`
- `n == land[i].length`
- `1` <= m, n <= `300`
- `land` consists of only `0'`s and `1'`s.
- Groups of farmland are rectangular in shape.

## Solution

```cpp
class Solution {
public:
    void dfs(int i, int j, int& r1, int& c1, int& r2, int& c2,vector<vector<int>>& land){
        if(i < 0 || j < 0 || i >= land.size() || j >= land[0].size() || land[i][j] != 1)
                return;
        land[i][j] = 0;
        r1=min(r1, i), c1=min(c1, j), r2=max(r2, i), c2=max(c2, j);
            dfs(i+1,j, r1, c1, r2, c2, land);
            dfs(i,j+1, r1, c1, r2, c2, land);
            // No need to check top and left because it is given that it is always a rectangle.
            //dfs(i-1,j, r1, c1, r2, c2, land);
            //dfs(i,j-1, r1, c1, r2, c2, land);

    }
    vector<vector<int>> findFarmland(vector<vector<int>>& land) {
        std::ios::sync_with_stdio(false);
        int n=land.size();
        int m=land[0].size();
        int r1,r2,c1,c2;
        vector<vector<int>> answer;
        for(int i=0; i<n; i++)
            for(int j=0; j<m; j++){
                if(land[i][j]==1){
                    r1=i, c1=j, r2=i, c2=j;
                    dfs(i, j, r1, c1, r2, c2, land);
                    answer.push_back({r1, c1, r2, c2});
                }
            }
        return answer;
    }
};
```

## Complexity

- `Time complexity : O(n*m)`
- `Space complexity : O(n*m)`

## Explaination

1. We will use the DFS technique to find the farmland.
2. This is very similar to the number of islands problem. [Check it Out](https://kts-o7.github.io/posts/problem-number-of-islands/).
3. We will iterate over the matrix given only if the current cell is `1`.
4. In the `dfs` function, we will check if the current cell is in bounds and is `1`.
5. If it is, we will mark it as `0` and update the `r1`, `c1`, `r2`, `c2` values.
6. The `r1`, `c1` will be the minimum values of the current cell and the `r2`, `c2` will be the maximum values of the current cell.
7. `r1`, `c1` will be the top left corner and `r2`, `c2` will be the bottom right corner of the farmland.
8. We will call the `dfs` function recursively on the right and bottom cells.
9. We need not to check the top and left cells because it is given that the farmland is always a rectangle.
10. Once the `dfs` function is ended, we will push the `r1`, `c1`, `r2`, `c2` values to the answer array.

---

> This demonstrates the use of DFS to traverse a 2D matrix and find the components or groups.
