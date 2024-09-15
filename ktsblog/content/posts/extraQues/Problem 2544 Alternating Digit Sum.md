+++
title = 'Problem 2544 Alternating Digit Sum'
date = 2024-09-15T16:00:34+05:30
draft = false
series = 'leetcode'
tags =['math','string']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2544](https://leetcode.com/problems/alternating-digit-sum/description/)

## Question

You are given a positive integer `n`. Each digit of `n` has a sign according to the following rules:

- The most significant digit is assigned a positive sign.
- Each other digit has an opposite sign to its adjacent digits.

Return the sum of all digits with their corresponding sign.

## Example 1

```
Input: n = 521
Output: 4
Explanation: (+5) + (-2) + (+1) = 4.
```

## Example 2

```
Input: n = 111
Output: 1
Explanation: (+1) + (-1) + (+1) = 1.
```

## Example 3

```
Input: n = 886996
Output: 0
Explanation: (+8) + (-8) + (+6) + (-9) + (+9) + (-6) = 0.
```

## Constraints

- 1<= `n` <= 10<sup>9</sup>

## Solution

```cpp
class Solution {
public:
    int alternateDigitSum(int n) {
        string num = to_string(n);
        int var = 0, sign = 1;
        for(int i = 0; i< num.size(); i++){
            var += (num[i]-'0')*sign;
            sign*=-1;
        }
        return var;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm         | Time Complexity | Space Complexity |
| ----------------- | --------------- | ---------------- |
| String conversion | O(log(n))       | O(1)             |
| Computation       | O(log(n))       | O(1)             |
```

## Explanation

### 1. Intuition

- To handle the MSB condition, convert to string and then process the string.

### 2. Implementation

```markdown
- Initialize the variable `var` to `0` and `sign` to `1`.
- Convert the integer to string.
- Iterate over the string and add the digit to `var` with the sign.
- Change the sign after each digit.
- Return the `var`.
```
