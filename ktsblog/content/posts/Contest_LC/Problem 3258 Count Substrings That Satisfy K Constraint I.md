+++
title = 'Problem 3258 Count Substrings That Satisfy K Constraint I'
date = 2024-08-18T22:13:46+05:30
draft = false
series = 'leetcode'
tags =[]
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3258](https://leetcode.com/problems/count-substrings-that-satisfy-k-constraint-i/description/)

## Question

You are given a binary string `s` and an integer `k`.

A binary string satisfies the k-constraint if either of the following conditions holds:

- The number of `0`'s in the string is at most `k`.
- The number of `1`'s in the string is at most `k`.

Return an integer denoting the number of
substrings of `s` that satisfy the k-constraint.

## Example 1

```
Input: s = "10101", k = 1

Output: 12

Explanation:

Every substring of `s` except the substrings `"1010"`, `"10101"`
and `"0101"` satisfies the k-constraint.
```

## Example 2

```
Input: s = "1010101", k = 2

Output: 25

Explanation:

Every substring of `s` except the substrings with a length greater than 5 satisfies the k-constraint.
```

## Example 3

```
Input: s = "11111", k = 1

Output: 15

Explanation:

All substrings of `s` satisfy the k-constraint.
```

## Constraints

```markdown
- `1 <= s.length <= 50`
- `1 <= k <= s.length`
- `s[i]` is either `'0'` or `'1'`.
```

## Solution

```cpp
class Solution {
public:
    int countKConstraintSubstrings(string s, int k) {
        int n = s.length();
        int count = 0;

        for (int i = 0; i < n; i++) {
            int zeros = 0, ones = 0;
            for (int j = i; j < n; j++) {
                if (s[j] == '0')
                    zeros++;
                else
                    ones++;

                if (zeros <= k || ones <= k) {
                    count++;
                } else {
                    break;
                }
            }
        }

        return count;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm   | Time Complexity | Space Complexity |
| ----------- | --------------- | ---------------- |
| Brute force | O(n^2)          | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- Since constraints are small we can check all possible substrings.
- For each substring, we can check if it satisfies the k-constraint.
- If it satisfies the k-constraint, we increment the count.
- At the end of the loop, we return the count.
```

### 2. Implementation

```markdown
- Intialize a variable `count` to store the number of substrings that satisfy the k-constraint.
- Iterate over the string `s` using two nested loops.
- For `i` from `0` to `n-1` do
  - Intialize two variables `zeros` and `ones` to store the number of `0`'s and `1`'s in the substring.
    - For `j` from `i` to `n-1` do
      - If `s[j]` is `0` increment `zeros` by `1`.
      - Else increment `ones` by `1`.
      - If `zeros` is less than or equal to `k` or `ones` is less than or equal to `k` increment `count` by `1`.
      - Else break the loop.
- Return the `count`.
```
