+++
title = 'Problem 861 Score After Flipping Matrix'
date = 2024-05-13T20:20:37+05:30
draft = false
series = 'leetcode'
tags =['matrix','greedy','bit-manipulation','cpp']
toc = false
math = true
+++

# Problem Statement

**Link** - [Problem 861](https://leetcode.com/problems/score-after-flipping-matrix/description)

## Question

You are given an `m x n` binary matrix `grid`.

A move consists of choosing any row or column and toggling each value in that row or column (i.e., changing all `0`'s to `1`'s, and all `1`'s to `0`'s).

Every row of the matrix is interpreted as a binary number, and the **score** of the matrix is the sum of these numbers.

Return the highest possible **score** after making any number of _moves_ (including zero moves).

## Example 1

{{< math.inline >}}

<p>
$$
\left[ \begin{array}{cccc}
0 & 0 & 1 & 1 \\
1 & 0 & 1 & 0 \\
1 & 1 & 0 & 0 \\
\end{array} \right]
\rightarrow
\left[ \begin{array}{cccc}
1 & 1 & 0 & 0 \\
1 & 0 & 1 & 0 \\
1 & 1 & 0 & 0 \\
\end{array} \right]
$$
$$
\left[ \begin{array}{cccc}
1 & 1 & 1 & 0 \\
1 & 0 & 0 & 0 \\
1 & 1 & 1 & 0 \\
\end{array} \right]
\rightarrow
\left[ \begin{array}{cccc}
1 & 1 & 1 & 1 \\
1 & 0 & 0 & 1 \\
1 & 1 & 1 & 1 \\
\end{array} \right]
$$
</p>
{{< /math.inline >}}

```text
Input: grid = [[0,0,1,1],[1,0,1,0],[1,1,0,0]]
Output: 39
Explanation: 0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39
```

## Example 2

```text
Input: grid = [[0]]
Output: 1
```

## Constraints

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 20`
- `grid[i][j]` is either `0` or `1`.

## Solution

```cpp
class Solution {
public:
    int matrixScore(vector<vector<int>>& grid) {

        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        for(int row =0; row<grid.size(); row++)
        {
            if(grid[row][0] == 0)
            {
                for(int col = 0; col<grid[0].size(); col++)
                {
                    grid[row][col] = !grid[row][col];
                }
            }
        }

        int zero = 0, one = 0;

        for(int col=0; col<grid[0].size(); col++)
        {
            for(int row=0; row<grid.size(); row++)
            {
                if(grid[row][col] == 0)
                    zero++;
                else
                    one++;
            }
 vector<int>zeroCnt(grid[0].size(),0);
        vector<int>oneCnt(grid[0].size(),0);
            if(zero > one )
            {
                for(int row=0; row<grid.size(); row++)
                {
                    grid[row][col] = !grid[row][col];
                }
            }
            one = 0;
            zero = 0;
        }

        int answer = 0;
        int temp,base;
         for(int row = 0; row<grid.size(); row++)
         {
            temp = 0;
            base = 0;
            for(int col = grid[0].size()-1;  col>=0; col--)
            {
                if(grid[row][col])
                {
                    temp+= (int)pow(2,base);
                }
                base++;
            }
            answer+= temp;
         }
         return answer;


    }
};
```

## Complexity Analysis

- `Time Complexity` - O(n^2)
- `Space Complexity` - O(1)

## Explanation

### 1. Intuition

- The goal is to maximize the final score of the matrix ie. maximize the sum of the binary numbers formed by the rows of the matrix.
- We can make a binary number maximum by making the most significant bit of the number 1.
- Example : `0111` is always less than `1000`.
- Then we can greedily make the other bits of the number into 1 provided it increases the sum of the binary numbers formed by the rows of the matrix.
- We can iterate over the columns and check if the number of `0's` in the column is greater than the number of `1's` then we can flip the column.
- This flipping is guranteed to increase the sum of the binary numbers formed by the rows of the matrix.
- Example : `[[1,0,1],[1,0,0]]`, if we flip 2nd column then the matrix becomes `[[1,1,1],[1,1,0]]` and the sum of the binary numbers formed by the rows of the matrix is `7+6 = 13` which is greater than `5+4 = 9`.

### 2. Implementation

- Iterate over the rows of the matrix and if the first element of the row is `0` then flip the row.
- Iterate over the columns of the matrix and if the number of `0's` in the column is greater than the number of `1's` then flip the column.
- Calculate the sum of the binary numbers formed by the rows of the matrix and return the sum.

---

> Shows the implementation of greedy technique to maximize the sum of the binary numbers formed by the rows of the matrix.
