+++
title = 'Problem 476 Number Complement'
date = 2024-08-22T10:49:10+05:30
draft = false
series = 'leetcode'
tags =['bit-manipulation']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 476](https://leetcode.com/problems/number-complement/description/)

## Question

The complement of an integer is the integer you get when you flip all the `0`'s to `1`'s and all the `1`'s to `0`'s in its binary representation.

- For example, The integer `5` is `"101"` in binary and its complement is `"010"` which is the integer `2`.

Given an integer `num`, return its complement.

## Example 1

```
Input: num = 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.
```

## Example 2

```
Input: num = 1
Output: 0
Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.
```

## Constraints

```markdown
- `1 <= num < 2^31`
```

## Solution

```cpp
class Solution {
public:
    int findComplement(int num) {
        int ans = num;
        int bitcnt = 0;
        while(num>0){
            num = num>>1;
            bitcnt++;
        }
        //cout<<bitcnt<<endl;
        //ans = ans^((1LL<<bitcnt)-1);     // to avoid Integer overflow we use 1LL ( long long 1 ) so that huge numbers can be handled when bit shifted.
        ans = ans^(int)(pow(2,bitcnt)-1);
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm        | Time Complexity | Space Complexity |
| ---------------- | --------------- | ---------------- |
| Bit manipulation | O(log2(n))      | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- To get the complement of a given number we first take its binary representation.
- Then we need to flip all the bits of the number.
- This can be achieved by doing the following
  1. Find the number of bits in the number.
  2. XOR it with all 1's of the same number of bits.
  3. This will generate the complement of given number
- This is due the fact of XOR operation

  - 0^0 = 0
  - 0^1 = 1
  - 1^0 = 1
  - 1^1 = 0
  - num ^ ~num = set bits of bitcount of num
    Hence `num ^ set bits of bitcount of num = ~num`

- Number of set bits of count `n` can be generated by `2^n - 1`

- Example
  if num = 5
  In binary 5 = 101
  bitcount = 3
  then
  number of set bits of 3 = 2^3 - 1 = 8 - 1 = 7 = 111
  101 ^ 111 = 010
  010 = 2
  Hence the complement is 2
```

### 2. Implementation

```markdown
- Initialize a variable `ans` with the value of `num`.
- Initialize a variable `bitcnt` with the value of 0.
- While `num` is greater than 0, do the following:
  - Right shift `num` by 1.
  - Increment `bitcnt` by 1.
- Calculate the complement of `num` using the following formula:
  - `ans = ans ^ (int)(pow(2, bitcnt) - 1)`.
- Return `ans`.
```