+++
title = 'Problem 3216 Lexographically Smallest String After a Swap'
date = 2024-08-15T19:15:18+05:30
draft = false
series = 'leetcode'
tags =['string','greedy']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3216](https://leetcode.com/problems/lexicographically-smallest-string-after-a-swap/description/)

## Question

Given a string `s` containing only digits, return the
lexicographically smallest string that can be obtained after swapping adjacent digits in `s` with the same parity at most once.

Digits have the same parity if both are odd or both are even. For example, 5 and 9, as well as 2 and 4, have the same parity, while 6 and 9 do not.

#### Lexicographically Smallest String:

A string `a` is lexicographically smaller than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`.
If the first `min(a.length, b.length)` characters do not differ, then the shorter string is the lexicographically smaller one.

## Example 1

```
Input: s = "45320"

Output: "43520"

Explanation:

s[1] == '5' and s[2] == '3' both have the same parity,
and swapping them results in the lexicographically smallest string.


```

## Example 2

```
Input: s = "001"

Output: "001"

Explanation:

There is no need to perform a swap because s is already the lexicographically smallest.
```

## Constraints

```markdown
- `2 <= s.length <= 100`
- `s consists only of digits.`
```

## Solution

```cpp
class Solution {
public:
    string getSmallestString(string s) {
        int n = s.length();
        for (int i = 0; i < n - 1; ++i) {
            if ((s[i] - '0') % 2 == (s[i + 1] - '0') % 2) {
                if (s[i] > s[i + 1]) {
                    swap(s[i], s[i + 1]);
                    break;
                }
            }
        }

        return s;
    }
};

```

## Complexity Analysis

```markdown
| Algorithm             | Time Complexity | Space Complexity |
| --------------------- | --------------- | ---------------- |
| Check parity and Swap | O(N)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- Iterate over the string and check if the current digit and the next digit have the same parity.
- If they have the same parity, then check if the current digit is greater than the next digit.
- If the current digit is greater than the next digit, then swap the digits and break the loop.
```

### 2. Implementation

```markdown
- Initialize a variable `n` to store the length of the string.
- Iterate over the string from index 0 to n-1.
- Check if the current digit and the next digit have the same parity.
- If they have the same parity, then check if the current digit is greater than the next digit.
- If the current digit is greater than the next digit, then swap the digits and break the loop.
- Return the modified string.
```
