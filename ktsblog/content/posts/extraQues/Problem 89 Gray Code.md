+++
title = 'Problem 89 Gray Code'
date = 2024-07-25T22:03:45+05:30
draft = false
series = 'leetcode'
tags =['math','bit-manipulation']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 89](https://leetcode.com/problems/gray-code/description/)

## Question

An n-bit gray code sequence is a sequence of 2^n integers where:

- `Every integer is in the inclusive range [0, 2^n - 1]`,
- `The first integer is 0`,
- `An integer appears no more than once in the sequence`,
- `The binary representation of every pair of adjacent integers differs by exactly one bit`, and
- `The binary representation of the first and last integers differs by exactly one bit`.

Given an integer n, return any valid n-bit gray code sequence.

## Example 1

```
Input: n = 2
Output: [0,1,3,2]
Explanation:
The binary representation of [0,1,3,2] is [00,01,11,10].
- 00 and 01 differ by one bit
- 01 and 11 differ by one bit
- 11 and 10 differ by one bit
- 10 and 00 differ by one bit
[0,2,3,1] is also a valid gray code sequence, whose binary representation is [00,10,11,01].
- 00 and 10 differ by one bit
- 10 and 11 differ by one bit
- 11 and 01 differ by one bit
- 01 and 00 differ by one bit
```

## Example 2

```
Input: n = 1
Output: [0,1]
```

## Constraints

```markdown
- `1 <= n <= 16`
```

## Solution

```cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        int size = 1<<n;
        vector<int>ans(size);

        for(int i = 0;i<size;i++)
            ans[i]=(i^(i>>1));
        return ans;

    }
};
```

## Complexity Analysis

```markdown
| Algorithm        | Time Complexity | Space Complexity |
| ---------------- | --------------- | ---------------- |
| Bit manipulation | O(2^N)          | O(2^N)           |
```

## Explanation

### 1. Intuition

```markdown
- We need to generate a gray code sequence of size 2^n.
- Since each number needs to be unique by one bit, we can use the XOR operator to generate the sequence.
- Given a number i, the gray code sequence is i^(i>>1).
- This is because the XOR operator will flip the bits that are different between the two numbers.
- hence we will take a number `i` and XOR it with `i>>1` to get the gray code sequence.
- This will generate a number that differs by one bit.
```

### 2. Implementation

```markdown
- Initialize a vector of size 2^n.
- Iterate from `0` to `2^n -1` and calculate the gray code sequence using the formula `i^(i>>1)`.
- Return the vector.
```

> Example

```
n = 3
size = 1<<3 = 8
ans = [0,0,0,0,0,0,0,0]
i = 0
ans[0] = 0^(0>>1) = 0^0 = 0
i = 1
ans[1] = 1^(1>>1) = 1^0 = 1
i = 2
ans[2] = 2^(2>>1) = 2^1 = 3
i = 3
ans[3] = 3^(3>>1) = 3^1 = 2
i = 4
ans[4] = 4^(4>>1) = 4^2 = 6
i = 5
ans[5] = 5^(5>>1) = 5^2 = 7
i = 6
ans[6] = 6^(6>>1) = 6^3 = 5
i = 7
ans[7] = 7^(7>>1) = 7^3 = 4
ans = [0,1,3,2,6,7,5,4]
```
