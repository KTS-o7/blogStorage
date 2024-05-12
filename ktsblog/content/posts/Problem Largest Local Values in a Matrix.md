+++
title = 'Problem Largest Local Values in a Matrix'
date = 2024-05-12T10:58:07+05:30
draft = false
series = 'leetcode'
tags =['matrix','2D array','cpp','vector']
toc = false
math = true
+++

# Problem Statement

**Link** - [Problem 2373](https://leetcode.com/problems/largest-local-values-in-a-matrix/description/)

## Question

You are given an `n x n` integer matrix `grid`.

Generate an integer matrix `maxLocal` of size `(n - 2) x (n - 2)` such that:

`maxLocal[i][j]` is equal to the largest value of the `3 x 3` matrix in grid centered around `row i + 1 and column j + 1`.
In other words, we want to find the largest value in every contiguous 3 x 3 matrix in grid.

Return the generated matrix.

## Example 1

{{< math.inline >}}

<p>

$$
Input : grid = \left[ \begin{array}{cccc}
   9 & 9 & 8 & 1 \\
5 & 6 & 2 & 6 \\
8 & 2 & 6 & 4 \\
6 & 2 & 2 & 2 \\
\end{array} \right]

Output : \left[ \begin{array}{cc}
   9 & 9 \\
8 & 6 \\
\end{array} \right]
$$

</p>
{{</ math.inline >}}

```text
Input: grid = [[9,9,8,1],[5,6,2,6],[8,2,6,4],[6,2,2,2]]
Output: [[9,9],[8,6]]
Explanation: The diagram above shows the original matrix and the generated matrix.
Each value in the generated matrix corresponds to the largest value of a contiguous 3 x 3 matrix in grid.
```

## Example 2

{{< math.inline >}}

<p>

$$
Input : grid = \left[ \begin{array}{cccccc}
   1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 \\
1 & 1 & 2 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 \\
\end{array} \right]

Output : \left[ \begin{array}{ccc}
2 & 2 & 2 \\
2 & 2 & 2 \\
2 & 2 & 2 \\
\end{array} \right]
$$

</p>

{{</ math.inline >}}

```text
Input: grid = [[1,1,1,1,1],[1,1,1,1,1],[1,1,2,1,1],[1,1,1,1,1],[1,1,1,1,1]]
Output: [[2,2,2],[2,2,2],[2,2,2]]
Explanation: Notice that the 2 is contained within every contiguous 3 x 3 matrix in grid.
```

## Constraints

- `n == grid.length == grid[i].length`
- `3 <= n <= 100`
- `1 <= grid[i][j] <= 100`

## Solution

```cpp
class Solution {
public:
    vector<vector<int>> largestLocal(vector<vector<int>>& grid) {
        vector<vector<int>>ans(grid.size()-2,vector<int>(grid[0].size()-2));
        int locMax = INT_MIN;
        for(int row = 0; row < grid.size()-2; row++)
        {
            for(int col = 0; col < grid[0].size()-2; col++)
            {
                for(int i = row; i<row+3; i++)
                {
                    for(int j = col; j<col+3; j++)
                        locMax = max(locMax,grid[i][j]);
                }
                ans[row][col] = locMax;
                locMax = INT_MIN;
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

- `Time Complexity` - O(n^2)
- `Space Complexity` - O(n^2)
- We are iterating over a 3x3 matrix each time thats `9` units and we have to visit each cell of the matrix so the time complexity is `9*n^2` which is `O(n^2)`.
- The space complexity is `O(n^2)` as we are storing the result in a 2D vector of size `(n-2) x (n-2)`.

## Explanation

### 1. Intuition

- We need to find the largest value in every contiguous 3 x 3 matrix in grid.
- We can iterate over the matrix and find the largest value in every 3 x 3 matrix.
- We can store the result in a new matrix and return it.
- To iterate over each contiguous 3 x 3 matrix, we need to iterate over the rows and columns of the matrix.

### 2. Implementation

- Initialize a 2D vector `ans` of size `(n-2) x (n-2)` to store the result.
- Initialize a variable `locMax` to store the maximum value in the 3 x 3 matrix.
- Iterate over the rows from `0` to `n-2` and columns from `0` to `n-2`.
- This will give us the starting point of each 3 x 3 matrix.
- Iterate over the rows from `row` to `row+3` and columns from `col` to `col+3`.
- This will give us the 3 x 3 matrix.
- Find the maximum value in the 3 x 3 matrix and store it in `locMax`.
- Store the maximum value in the result matrix `ans[row][col]`.
- Reset `locMax` to `INT_MIN` for the next 3 x 3 matrix.
- Return the result matrix `ans`.

## Intresting fact

This problem closely simulates a very powerful ML algorithm in CNNs called as Max Pooling. Max pooling is a sample-based discretization process. The objective is to down-sample an input representation (image, hidden-layer output matrix, etc.), reducing its dimensionality and allowing for assumptions to be made about features contained in the sub-regions binned.

Key terms in Max Pooling:

- **Pooling Layer**: The pooling layer is a new layer added after the convolutional layer. It is used to reduce the spatial dimensions of the output volume.
- **Max Pooling**: Max pooling is a pooling operation that selects the maximum element from the region of the feature map covered by the filter. Thus, the output after max-pooling is the maximum value of the region covered by the filter.
- **Filter**: The filter is a matrix that is used to extract features from the input image. The filter is also known as a kernel. The filter is applied to the input image to produce the feature map. **In this case, the filter is of size 3x3.**
- **Stride**: The stride is the number of pixels by which the filter is moved over the input image. The stride is used to reduce the spatial dimensions of the output volume. **In this case, the stride is 1.**

![placeholder](https://production-media.paperswithcode.com/methods/MaxpoolSample2.png)

---

> This problem simulates Max pooling. This shows a real world application of a DSA problem.
