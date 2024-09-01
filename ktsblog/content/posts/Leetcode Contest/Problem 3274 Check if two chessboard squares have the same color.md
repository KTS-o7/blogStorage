+++
title = 'Problem 3274 Check if Two Chessboard Squares Have the Same Color'
date = 2024-09-01T16:56:04+05:30
draft = false
series = 'leetcode'
tags =['simulation','math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3274](https://leetcode.com/problems/check-if-two-chessboard-squares-have-the-same-color/description/)

## Question

You are given two strings, `coordinate1` and `coordinate2`, representing the coordinates of a square on an `8 x 8` chessboard.

Below is the chessboard for reference.

![imgChessBoard](https://assets.leetcode.com/uploads/2024/07/17/screenshot-2021-02-20-at-22159-pm.png)

Return `true` if these two squares have the same color and `false` otherwise.

The coordinate will always represent a valid chessboard square. The coordinate will always have the letter first (indicating its column), and the number second (indicating its row).

## Example 1

```
Input: coordinate1 = "a1", coordinate2 = "c3"

Output: true

Explanation:

Both squares are black.
```

## Example 2

```
Input: coordinate1 = "a1", coordinate2 = "h3"

Output: false

Explanation:

Square "a1" is black and "h3" is white.
```

## Constraints

```markdown
- `coordinate1.length == coordinate2.length == 2`
- `'a' <= coordinate1[0], coordinate2[0] <= 'h'`
- `'1' <= coordinate1[1], coordinate2[1] <= '8'`
```

## Solution

```cpp
class Solution {
public:
    bool checkTwoChessboards(string c1, string c2) {
        int col1 = (c1[0] - 'a' + 1 + c1[1] - '0') % 2;

        int col2 = (c2[0] - 'a' + 1 + c2[1] - '0') % 2;

        return col1 == col2;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm  | Time Complexity | Space Complexity |
| ---------- | --------------- | ---------------- |
| Simulation | O(1)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- From the board we can see that `a,c,e,g` are black and `b,d,f,h` are white.
- Similarly `1,3,5,7` are black and `2,4,6,8` are white.
- If we assign black as `0` and white as `1`
  then we can see that the sum of the column and row is `even` for black and `odd` for white.
- So given each cordinate, convert it into integer and then find remiander when divided by `2`.
- Then we need to compare the remainder of both the cordinates.
```

### 2. Implementation

```markdown
- A cordinate is given in the form of `a1` where `a` is the column and `1` is the row.
- We need to convert the column and row into integer.
- intger value is calculated as `(column value+1) + row value`.
- Then we find the remainder when divided by `2`.
- We then compare the remainder of both the cordinates.
- Return `true` if both the remainders are same and `false` otherwise.
```
