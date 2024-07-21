+++
title = 'Problem 1380 Lucky Number in a Matrix'
date = 2024-07-19T21:38:03+05:30
draft = false
series = 'leetcode'
tags =['matrix','maxi-min','mini-max']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1380](https://leetcode.com/problems/lucky-numbers-in-a-matrix/description/)

## Question

Given an `m x n` matrix of distinct numbers, return all lucky numbers in the matrix in any order.

A `lucky number` is an element of the matrix such that it is the minimum element in its row and maximum in its column.

## Example 1

```
Input: matrix = [[3,7,8],[9,11,13],[15,16,17]]
Output: [15]
Explanation: 15 is the only lucky number
since it is the minimum in its row and the maximum in its column.
```

## Example 2

```
Input: matrix = [[1,10,4,2],[9,3,8,7],[15,16,17,12]]
Output: [12]
Explanation: 12 is the only lucky number
since it is the minimum in its row and the maximum in its column.
```

## Example 3

```
Input: matrix = [[7,8],[1,2]]
Output: [7]
Explanation: 7 is the only lucky number
since it is the minimum in its row and the maximum in its column.
```

## Constraints

```markdown
- `m == mat.length`
- `n == mat[i].length`
- `1 <= n, m <= 50`
- `1 <= matrix[i][j] <= 10^5.`
- All elements in the matrix are distinct.
```

## Solution

```cpp
class Solution {
public:
    vector<int> luckyNumbers (vector<vector<int>>& matrix) {
        vector<int>rowMin(matrix.size(),INT_MAX);
        vector<int>colMax(matrix[0].size(),INT_MIN);
        int rows = matrix.size();
        int cols = matrix[0].size();

        for(int i = 0; i<rows; i++){
            for(int j = 0; j<cols ; j++){
                rowMin[i] = min(rowMin[i], matrix[i][j]);
                colMax[j] = max(colMax[j],matrix[i][j]);
            }
        }
        /*for(auto it:rowMin)
            cout<<it<<" ";
        cout<<'\n'<<endl;
        for(auto it:colMax)
            cout<<it<<" ";*/
        vector<int>ans;
        for(int i = 0; i<rows; i++){
            for(int j=0; j<cols; j++){
                if(matrix[i][j] == rowMin[i] && matrix[i][j]==colMax[j]){
                    ans.push_back(matrix[i][j]);
                }
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm        | Time Complexity | Space Complexity |
| ---------------- | --------------- | ---------------- |
| Matrix Traversal | O(N^2)          | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- We can store the row minimums and column maximums in two separate arrays.
- Then we can check the each element of the matrix
  with the row minimum and column maximum of that row and column.
- If the element is equal to both the row minimum and column maximum,
  then it is a lucky number.
- We can store all the lucky numbers in a vector and return it.
```

### 2. Implementation

```markdown
- Initialize two vectors `rowMin` and `colMax` with INT_MAX and INT_MIN respectively.
- Traverse the matrix and update the `rowMin` and `colMax` vectors.
- Traverse the matrix again and check if the element is equal to the row minimum and column maximum.
- If it is, then push the element into the answer vector.
- Return the answer vector.
```

### Alternate Approach

There is a more optimized approach to solve this problem.
We can develop a Constant Space Complexity solution.
But before that we need to prove that there is only one lucky number in the matrix.

#### Proof by Contradiction

```
- Suppose we have an integer `X` which is the minimum in row `r1` and maximum in column `c1`.
- Lets say there is another integer `Y` which is the minimum in row `r2` and maximum in column `c2`.
```

![image](https://leetcode.com/problems/lucky-numbers-in-a-matrix/Figures/1380/1380A.png)

```
Hence we can conclude that there can be only one lucky number in the matrix.
```

```markdown
- We can take advantage of the above fact as follows
  - The lucky number will always be the maximum of the minimums of Row
    and the minimum of the maximums of Column.
- This is because every element is **unique**

**Algorithm**:

1. iterate over each row and find the minimum element in that row.
2. then find the maximum of all the minimums.
3. iterate over each column and find the maximum element in that column.
4. then find the minimum of all the maximums.
5. if the maximum of minimums is equal to the minimum of maximums,
   then that element is the lucky number.
6. return the vector.
```

#### Code

```cpp
class Solution {
public:
    vector<int> luckyNumbers (vector<vector<int>>& matrix) {
        int N = matrix.size(), M = matrix[0].size();

        int rMinMax = INT_MIN;
        for (int i = 0; i < N; i++) {

            int rMin = INT_MAX;
            for (int j = 0; j < M; j++) {
                rMin = min(rMin, matrix[i][j]);
            }
            rMinMax = max(rMinMax, rMin);
        }

        int cMaxMin = INT_MAX;
        for (int i = 0; i < M; i++) {

            int cMax = INT_MIN;
            for (int j = 0; j < N; j++) {
                cMax = max(cMax, matrix[j][i]);
            }
            cMaxMin = min(cMaxMin, cMax);
        }

        if (rMinMax == cMaxMin) {
            return {rMinMax};
        }

        return {};
    }
};
```
