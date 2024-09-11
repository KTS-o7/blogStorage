+++
title = 'Problem 2220 Minimum Bit Flips to Convert Number'
date = 2024-09-11T09:20:56+05:30
draft = false
series = 'leetcode'
tags =['bit-manipulation','xor','count','hamming-distance']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2220](https://leetcode.com/problems/minimum-bit-flips-to-convert-number/description/)

## Question

A bit flip of a number `x` is choosing a bit in the binary representation of `x` and flipping it from either `0` to `1` or `1` to `0`.

For example, for `x = 7`, the binary representation is `111` and we may choose any bit (including any leading zeros not shown) and flip it. We can flip the first bit from the right to get `110`, flip the second bit from the right to get `101`, flip the fifth bit from the right (a leading zero) to get `10111`, etc.
Given two integers `start` and `goal`, return the minimum number of bit flips to convert `start` to `goal`

## Example 1

```
Input: start = 10, goal = 7
Output: 3
Explanation: The binary representation of 10 and 7 are 1010 and 0111 respectively.
We can convert 10 to 7 in 3 steps:
- Flip the first bit from the right: 1010 -> 1011.
- Flip the third bit from the right: 1011 -> 1111.
- Flip the fourth bit from the right: 1111 -> 0111.
It can be shown we cannot convert 10 to 7 in less than 3 steps. Hence, we return 3.
```

## Example 2

```
Input: start = 3, goal = 4
Output: 3
Explanation: The binary representation of 3 and 4 are 011 and 100 respectively.
We can convert 3 to 4 in 3 steps:
- Flip the first bit from the right: 011 -> 010.
- Flip the second bit from the right: 010 -> 000.
- Flip the third bit from the right: 000 -> 100.
It can be shown we cannot convert 3 to 4 in less than 3 steps. Hence, we return 3.

```

## Constraints

- 1<= `start`, `goal` <= 10<sup>9</sup>

## Solution

```cpp
// Bruteforce - check each bit and count if not equal
class Solution {
public:
    int minBitFlips(int start, int goal) {
        int count = 0;
        while (start > 0 || goal > 0) {
            if ((start & 1) != (goal & 1)) {
                count++;
            }
            start >>= 1;
            goal >>= 1;
        }
        return count;
    }
};

// Optimized - XOR and count set bits
class Solution {
    public:
    int minBitFlips(int start, int goal) {
        int count = 0;
        for (int x = start ^ goal; x > 0; x >>= 1) {
            count += x & 1;
        }
        return count;
    }
};

// Using built-in function
class Solution {
    public:
    int minBitFlips(int start, int goal) {
        return __builtin_popcount(start ^ goal);
    }
};
```

## Complexity Analysis

```markdown
| Algorithm  | Time Complexity               | Space Complexity |
| ---------- | ----------------------------- | ---------------- |
| Bruteforce | O(max(log(start), log(goal))) | O(1)             |
| Optimized  | O(log(start ^ goal))          | O(1)             |
| Built-in   | O(1)                          | O(1)             |
```

## Explanation

### 1. Intuition

The problem is asking to find the minimum number of bit flips to convert `start` to `goal`.
In other words, we need to find the number of bits which are different in `start` and `goal`.

- XOR operation on two numbers will provide `1` at the bit position where the bits are different and `0` where they are the same.
- XOR operation on input numbers will produce a new number with `1` at the bit position where the bits are different.
- If we count the number of `1`s in the new number, it will give the number of bits that are different in the input numbers.

### 2. Implementation

```markdown
1. Initialize a variable `count` to `0`.
2. Iterate over the XOR of `start` and `goal` until it becomes `0`.
   - If the last bit of the XOR is `1`, increment the `count`.
   - Shift the XOR to the right by `1`.
3. Return the `count`.
```

> This problem is exactly the same as counting the Hamming distance between two numbers.

> Leetcode Hamming Distance - [Problem 461](https://leetcode.com/problems/hamming-distance/description/)
