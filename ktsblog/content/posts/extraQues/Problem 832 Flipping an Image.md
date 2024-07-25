+++
title = 'Problem 832 Flipping an Image'
date = 2024-07-25T21:38:05+05:30
draft = false
series = 'leetcode'
tags =['vector','array','matrix']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 832](https://leetcode.com/problems/flipping-an-image/description/)

## Question

Given an `n x n` binary matrix `image`, flip the image horizontally, then invert it, and return the resulting image.

To flip an image horizontally means that each row of the image is reversed.

- For example, flipping `[1,1,0]` horizontally results in `[0,1,1]`.
  To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0.

- For example, inverting `[0,1,1]` results in `[1,0,0]`.

## Example 1

```
Input: image = [[1,1,0],[1,0,1],[0,0,0]]
Output: [[1,0,0],[0,1,0],[1,1,1]]
Explanation: First reverse each row: [[0,1,1],[1,0,1],[0,0,0]].
Then, invert the image: [[1,0,0],[0,1,0],[1,1,1]]
```

## Example 2

```
Input: image = [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
Output: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
Explanation: First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].
Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
```

## Constraints

```markdown
- `n == image.length`
- `n == image[i].length`
- `1 <= n <= 20`
- `images[i][j] is either 0 or 1`.
```

## Solution

```cpp
class Solution {
public:
    vector<vector<int>> flipAndInvertImage(vector<vector<int>>& matrix) {
        int rows = matrix.size();
        int cols = matrix[0].size();
        vector<vector<int>>v(rows,vector<int>(cols));
        for(int i = 0; i<rows; i++){
            for(int j = 0; j<cols; j++){
                if(matrix[i][j]==1)
                    v[i][cols-1-j] = 0;
                else
                    v[i][cols-1-j] = 1;
            }
        }
        return v;

    }
};
```

## Complexity Analysis

```markdown
| Algorithm        | Time Complexity | Space Complexity |
| ---------------- | --------------- | ---------------- |
| Matrix Traversal | O(N^2)          | O(N^2)           |
```

## Explanation

### 1. Intuition

```markdown
- Just simulate what the question is asking.
- First reverse the each row and then invert the elements.
- But here we are creating a new matrix to store the result.
```

### 2. Implementation

```markdown
- Initialize `rows` and `cols` to store the number of rows and columns.
- Create a new matrix `v` to store the result.
- Traverse through the matrix and do the following
  - If the element is 1 then set the element at `i`th row and `cols-1-j`th column to 0
  - Else set the element at `i`th row and `cols-1-j`th column to 1
- Return the result matrix.
```
