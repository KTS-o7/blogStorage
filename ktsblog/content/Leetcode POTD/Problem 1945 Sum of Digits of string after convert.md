+++
title = 'Problem 1945 Sum of Digits of String After Convert'
date = 2024-09-03T13:14:25+05:30
draft = false
series = 'leetcode'
tags =['string','math','manipulation']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1945](https://leetcode.com/problems/sum-of-digits-of-string-after-convert/description/)

## Question

You are given a string `s` consisting of lowercase English letters, and an integer `k`.

First, convert `s` into an integer by replacing each letter with its position in the alphabet (i.e., replace `'a'` with `1`, `'b'` with `2`, ..., `'z'` with `26`). Then, transform the integer by replacing it with the sum of its digits. Repeat the transform operation `k` times in total.

For example, if `s = "zbax"` and `k = 2`, then the resulting integer would be `8` by the following operations:

- Convert: `"zbax" ➝ "(26)(2)(1)(24)" ➝ "262124" ➝ 262124`
- Transform #1: `262124 ➝ 2 + 6 + 2 + 1 + 2 + 4 ➝ 17`
- Transform #2: `17 ➝ 1 + 7 ➝ 8`

Return the resulting integer after performing the operations described above.

#### Note

NGL this question is way more easy than it sounds. They want us to first convert each character to its position in the alphabet and then sum the digits of the resulting number. We have to do this `k` times.
After first run it will be a set of digits. Then we need to repeatedly sum the digits of the number until we get a single digit or we exceed `k` times.

## Example 1

```
Input: s = "iiii", k = 1
Output: 36
Explanation: The operations are as follows:
- Convert: "iiii" ➝ "(9)(9)(9)(9)" ➝ "9999" ➝ 9999
- Transform #1: 9999 ➝ 9 + 9 + 9 + 9 ➝ 36
Thus the resulting integer is 36.
```

## Example 2

```
Input: s = "leetcode", k = 2
Output: 6
Explanation: The operations are as follows:
- Convert: "leetcode" ➝ "(12)(5)(5)(20)(3)(15)(4)(5)" ➝ "12552031545" ➝ 12552031545
- Transform #1: 12552031545 ➝ 1 + 2 + 5 + 5 + 2 + 0 + 3 + 1 + 5 + 4 + 5 ➝ 33
- Transform #2: 33 ➝ 3 + 3 ➝ 6
Thus the resulting integer is 6.
```

## Example 3

```
Input: s = "zbax", k = 2
Output: 8
```

## Constraints

- `1 <= s.length <= 100`
- `1 <= k <= 10`
- `s` consists of lowercase English letters.

## Solution

```cpp
class Solution {
public:
    int getLucky(string s, int k) {
        string number = "";
        for (char& x : s) {
            number += to_string(x - 'a' + 1);
        }
        int temp;
        while (k > 0) {
            temp = 0;
            for (char& x : number) {
                temp += x - '0';
            }
            number = to_string(temp);
            k--;
        }
        return stoi(number);
    }
};
```

## Complexity Analysis

```markdown
| Algorithm        | Time Complexity | Space Complexity |
| ---------------- | --------------- | ---------------- |
| String traversal | O(n)            | O(1)             |
| Repeated sum     | O(kn)           | O(1)             |
| Total            | O(n+kn)         | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- The idea is to first convert the string to a number by replacing each character with its position in the alphabet.
- Then we need to sum the digits of the number.
- We need to repeat this process `k` times.
- We can do this by converting the number to a string and then summing the digits.
- After each sum, we need to convert the number back to a string.
- After `k` iterations, we return the number.
```

### 2. Implementation

```markdown
- Initialize a string `number` to store the number.
- Iterate over the string and convert each character to its position in the alphabet using `to_string(x - 'a' + 1)`.
- Initialize a variable `temp` to store the sum of the digits.
- While `k` is greater than `0`
  - Initialize `temp` to `0`.
  - Iterate over the string
    - `temp += x - '0'`.
  - Convert `temp` to a string and store it in `number`.
  - Decrement `k`.
- Return the integer value of `number`.
```
