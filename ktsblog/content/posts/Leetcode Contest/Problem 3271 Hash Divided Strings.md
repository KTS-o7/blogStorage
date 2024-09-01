+++
title = 'Problem 3271 Hash Divided Strings'
date = 2024-09-01T16:22:19+05:30
draft = false
series = 'leetcode'
tags =['string','hashing']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3271](https://leetcode.com/problems/hash-divided-string/description/)

## Question

You are given a string `s` of length `n` and an integer `k`, where `n` is a multiple of `k`. Your task is to hash the string `s` into a new string called `result`, which has a length of `n / k`.

First, divide `s` into `n / k` substrings , each with a length of `k`. Then, initialize result as an empty string.

For each substring in order from the beginning:

- The hash value of a character is the index of that character in the English alphabet (e.g., `'a' → 0`, `'b' → 1`, ..., `'z' → 25`).
- Calculate the sum of all the hash values of the characters in the substring.
- Find the remainder of this sum when divided by 26, which is called `hashedChar`.
- Identify the character in the English lowercase alphabet that corresponds to `hashedChar`.
- Append that character to the end of `result`.

Return `result`.

> A substring is a contiguous non-empty sequence of characters within a string.

## Example 1

```
Input: s = "abcd", k = 2

Output: "bf"

Explanation:

First substring: "ab", 0 + 1 = 1, 1 % 26 = 1, result[0] = 'b'.

Second substring: "cd", 2 + 3 = 5, 5 % 26 = 5, result[1] = 'f'.
```

## Example 2

```
Input: s = "mxz", k = 3

Output: "i"

Explanation:

The only substring: "mxz", 12 + 23 + 25 = 60, 60 % 26 = 8, result[0] = 'i'.
```

## Constraints

```markdown
- `1 <= k <= 100`
- `k <= s.length <= 1000`
- `s.length` is divisible by `k`.
- `s` consists only of lowercase English letters.
```

## Solution

```cpp
class Solution {
public:
    string stringHash(string s, int k) {
        std::ios::sync_with_stdio(false);
        int n = s.length();
        int subcnt = n / k;
        string result = "";

        for (int i = 0; i < subcnt; i++) {
            int sum = 0;

            for (int j = 0; j < k; j++) {
                sum += s[i * k + j] - 'a';
            }

            int val = sum % 26;
            result += (char)(val + 'a');
        }

        return result;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm       | Time Complexity | Space Complexity |
| --------------- | --------------- | ---------------- |
| Hash generation | O(N)            | O(1)             |
```

## Explanation

### 1. Intuition

This question is also relatively simple. We need to do exactly what the question asks for.

- Then we need to chunk the string into substrings of length `k`.
- For each substring, we need to calculate the sum of the hash values of the characters.
- Then we need to find the remainder of this sum when divided by 26.
- Finally, we need to append the character corresponding to this remainder to the result string.

```markdown
- Calculate how many substrings we can form.
- That is equal to the length of the string divided by k.
- We need that many iterations.
- For each iteration, calculate the sum of the hash values of the characters.
- Hashvalue is simply the difference of the character and 'a'.
- Find the remainder of the sum when divided by 26.
- Append the character corresponding to this remainder to the result string.
```

### 2. Implementation

```markdown
- Declare a variable `n` to store the length of the string.
- Declare a variable `subcnt` to store the number of substrings.
- Declare a string `result` to store the result.
- `subcnt` = `n` / `k`.
- Iterate over from `0` to `subcnt`.
  - Declare a variable `sum` to store the sum of the hash values of the characters.
  - Iterate over from `0` to `k`.
    - Add the hash value of the character to the `sum`.
    - Hash value is `s[i * k + j] - 'a'`.
  - Find the remainder of the `sum` when divided by 26.
  - Append the character corresponding to this remainder to the `result`.
```
