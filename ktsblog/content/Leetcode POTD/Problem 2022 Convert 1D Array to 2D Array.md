+++
title = 'Problem 2022 Convert 1D Array to 2D Array'
date = 2024-09-01T17:21:31+05:30
draft = false
series = 'leetcode'
tags =['array','matrix','simulation']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2022](https://leetcode.com/problems/convert-1d-array-into-2d-array/description/)

## Question

You are given a 0-indexed 1-dimensional (1D) integer array `original`, and two integers, `m` and `n`. You are tasked with creating a 2-dimensional (2D) array with `m` rows and `n` columns using all the elements from `original`.

The elements from indices `0` to `n - 1` (inclusive) of `original` should form the first row of the constructed 2D array, the elements from indices `n` to `2 * n - 1` (inclusive) should form the second row of the constructed 2D array, and so on.

Return an `m x n` 2D array constructed according to the above procedure, or an `empty 2D array` if it is impossible.

## Example 1

```
Input: original = [1,2,3,4], m = 2, n = 2
Output: [[1,2],[3,4]]
Explanation: The constructed 2D array should contain 2 rows and 2 columns.
The first group of n=2 elements in original, [1,2],
becomes the first row in the constructed 2D array.
The second group of n=2 elements in original, [3,4],
becomes the second row in the constructed 2D array.
```

## Example 2

```
placeHoldeInput: original = [1,2,3], m = 1, n = 3
Output: [[1,2,3]]
Explanation: The constructed 2D array should contain 1 row and 3 columns.
Put all three elements in original into the first row of the constructed 2D array.
```

## Example 3

```
Input: original = [1,2], m = 1, n = 1
Output: []
Explanation: There are 2 elements in original.
It is impossible to fit 2 elements in a 1x1 2D array, so return an empty 2D array.
```

## Constraints

```markdown
- `1 <= original.length <= 5 * 10^4`
- `1 <= original[i] <= 10^5`
- `1 <= m, n <= 4 * 10^4`
```

## Solution

```cpp
class Solution {
public:
    vector<vector<int>> construct2DArray(vector<int>& original, int m, int n) {
        int size = original.size();
        if(size != m*n)
            return {};
        vector<vector<int>>ans(m,vector<int>(n));
        int idx = 0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                ans[i][j]=original[idx];
                idx++;
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm       | Time Complexity | Space Complexity |
| --------------- | --------------- | ---------------- |
| Array traversal | O(n)            | O(mn)            |
```

## Explanation

### 1. Intuition

The problem is about converting a 1-dimensional array into a 2-dimensional matrix with specified dimensions `m` (rows) and `n` (columns). The key observation is that for this conversion to be possible, the total number of elements in the 1D array must exactly match the number of elements in the desired 2D array (`m * n`). If they don't match, it is impossible to form the desired 2D array, and we should return an empty array.

Here's the step-by-step breakdown of the approach:

1. **Check the Feasibility:** First, we need to check if the number of elements in the original array is equal to `m * n`. If not, return an empty 2D array.

2. **Matrix Construction:** If the sizes match, we can proceed to fill the matrix row by row. Each row in the matrix will take `n` consecutive elements from the original array.

3. **Traversing the Original Array:** We simply iterate through the original array and place each element in its corresponding position in the 2D matrix. This is done by keeping track of an index that moves through the original array and fills the 2D array in row-major order.

### 2. Implementation

The implementation directly follows the intuition:

- We first check if the total number of elements in the original array equals `m * n`. If not, we return an empty 2D array.
- We initialize a 2D array with `m` rows and `n` columns.
- We then use nested loops to fill the 2D array, iterating through each element in the original array and placing it in the correct position in the matrix.
