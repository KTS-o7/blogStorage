+++
title = 'Problem 371 Sum of Two Integers'
date = 2024-07-29T22:05:47+05:30
draft = false
series = 'leetcode'
tags =['bit-manipulation']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 371](https://leetcode.com/problems/sum-of-two-integers/description/)

## Question

Given two integers `a` and `b`, return the sum of the two integers without using the operators + and -.

## Example 1

```
Input: a = 1, b = 2
Output: 3
```

## Example 2

```
Input: a = 2, b = 3
Output: 5
```

## Constraints

```markdown
- `-1000 <= a, b <= 1000`
```

## Solution

```cpp
class Solution {
public:
    int getSum(int a, int b) {
        if(b==0)
            return a;
        else
            return getSum(a^b,(a&b)<<1);
    }
};
```

## Complexity Analysis

```markdown
| Algorithm        | Time Complexity | Space Complexity |
| ---------------- | --------------- | ---------------- |
| Bit Manipulation | O(1)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- How can we add two numbers in binary form?
- To add two bits we can use XOR operation.
- This will handle the case where there is no carry being generated

  - Example: 2+1
  - 2 -> 10
  - 1 -> 01
  - 2^1 -> 11

- To handle the carry we can use AND operation.
  - Example: 3+2
  - 3-> 011
  - 2-> 010
  - 3^2 = 001 here carry goes missing hence we need to use and operator and shift it by 1
  - (3&2)<<1 = 100
  - so 3+2 = (3&2)<<1 ^ 3^2 = 100 ^ 001 = 101 = 5

We need to do this until there is no carry left.
```

### 2. Implementation

```markdown
- If `b` is 0 then return `a`
- Else return the sum of `a^b` and `(a&b)<<1`
```

### Iterative Approach

```cpp
class Solution {
    public int getSum(int a, int b) {
        int ans=0;
        int carry=0;
        while(b!=0){
            ans = a^b;
            carry = (a&b)<<1;
            a=ans;
            b=carry;
        }
        return a;
    }
}
```
