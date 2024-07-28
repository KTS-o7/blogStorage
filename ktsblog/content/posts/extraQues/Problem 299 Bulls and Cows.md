+++
title = 'Problem 299 Bulls and Cows'
date = 2024-07-28T20:46:25+05:30
draft = false
series = 'leetcode'
tags =['hash-map','string','frequency-count']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 299](https://leetcode.com/problems/bulls-and-cows/description/)

## Question

You are playing the **Bulls and Cows** game with your friend.

You write down a secret number and ask your friend to guess what the number is. When your friend makes a guess, you provide a hint with the following info:

- The number of "bulls", which are digits in the guess that are in the correct position.
- The number of "cows", which are digits in the guess that are in your secret number but are located in the wrong position. Specifically, the non-bull digits in the guess that could be rearranged such that they become bulls.

Given the secret number `secret` and your friend's guess `guess`, return the `hint` for your friend's `guess`.

The hint should be formatted as `"xAyB"`, where `x` is the number of bulls and `y` is the number of cows. Note that both secret and guess may contain duplicate digits.

## Example 1

```
Input: secret = "1807", guess = "7810"
Output: "1A3B"
Explanation: Bulls are connected with a '|' and cows are underlined:
"1807"
  |
"7810"
```

## Example 2

```
Input: secret = "1123", guess = "0111"
Output: "1A1B"
Explanation: Bulls are connected with a '|' and cows are underlined:
"1123"        "1123"
  |      or     |
"0111"        "0111"
Note that only one of the two unmatched 1s is counted as a cow
since the non-bull digits can only be rearranged to allow one 1 to be a bull.
```

## Constraints

```markdown
- `1 <= secret.length, guess.length <= 1000`
- `secret.length == guess.length`
- `secret` and `guess` consist of digits only.
```

## Solution

```cpp
class Solution {
public:
    string getHint(string secret, string guess) {
        vector<int> s(10, 0);
        vector<int> g(10, 0);
        int bull = 0, cow = 0;

        for (int i = 0; i < secret.size(); i++) {
            if (secret[i] == guess[i]) {
                bull++;
            } else {
                s[secret[i] - '0']++;
                g[guess[i] - '0']++;
            }
        }

        for (int i = 0; i < 10; i++) {
            cow += min(s[i], g[i]);
        }

        return to_string(bull) + 'A' + to_string(cow) + 'B';
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Traversal | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition

```markdown
- We can count the frequencies of digits in `secret` and `guess`.
- After that bulls will be the count of matching characters in given string.
- Cows will be the sum of minimum of frequencies of each digit in `secret` and `guess`.
- Sum of minimums works because
  - if the frequency of a digit `k` is less in guess then it means, `k` digits are present but in wrong places.
  - if the frequency of a digit `k` is less in secert it means only `k` digits are present but are in wrong places.
- return in `xAyB` format.
```

### 2. Implementation

```markdown
- Initialize two vectors `s` and `g` to count the frequencies of digits.
- Iterate over the strings
- if the characters are matching then increment the `bull` count.
- else increment the frequencies of digits in `s` and `g`.
- Iterate over the frequencies of digits and calculate the `cow` count.
  - cow will be the sum of minimum of frequencies of digits in `s` and `g`.
- return the `xAyB` format.
```
