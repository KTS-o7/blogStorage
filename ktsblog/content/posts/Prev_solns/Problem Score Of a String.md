+++
title = 'Problem 3110 Score of a String'
date = 2024-06-01T11:51:08+05:30
draft = false
series = 'leetcode'
tags =['string','traversal']
toc = false
+++

# Problem Statement

**Link** - [Problem 3110](https://leetcode.com/problems/score-of-a-string/description/)

## Question

You are given a string `s`. The score of a string is defined as the sum of the absolute difference between the ASCII values of adjacent characters.

Return the score of `s`.

## Example 1

```text
Input: s = "hello"

Output: 13

Explanation:
The ASCII values of the characters in s are: 'h' = 104, 'e' = 101, 'l' = 108, 'o' = 111. So, the score of s would be |104 - 101| + |101 - 108| + |108 - 108| + |108 - 111| = 3 + 7 + 0 + 3 = 13.
```

## Example 2

```text
Input: s = "zaz"

Output: 50

Explanation:
The ASCII values of the characters in s are: 'z' = 122, 'a' = 97. So, the score of s would be |122 - 97| + |97 - 122| = 25 + 25 = 50.
```

## Constraints

- `2 <= s.length <= 100`
- `s` consists only of lowercase English letters.

## Solution

```cpp
class Solution {
public:
    int scoreOfString(string s) {
        std::ios::sync_with_stdio(false);
        cout.tie(0);
        cin.tie(0);
        int score = 0;
        for(int i = 0;i<s.size()-1;i++)
        {
            score+= abs((int)(s[i]-s[i+1]));
        }
        return score;
    }
};
```

## Complexity Analysis

- `Time Complexity`: O(N)
- `Space Complexity`: O(1)

## Explanation

### 1. Intuition

- We can iterate over the string and calculate the score by taking the absolute difference between the ASCII values of adjacent characters.

### 2. Implementation

- Initialize the `score` to 0.
- Iterate over the string from `0` to `s.size()-1`.
- Calculate the score by taking the absolute difference between the ASCII values of adjacent characters.
- Return the score.

---

> This solution is a simple implementation of problem statement.
