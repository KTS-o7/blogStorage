+++
title = 'Problem Reverse Prefix of Word'
date = 2024-05-01T20:47:44+05:30
draft = false
series = 'Leetcode'
tags = ['leetcode', 'cpp', 'string']
toc = false
+++

# Problem Statement

**Link** - [Problem 2000](https://leetcode.com/problems/reverse-prefix-of-word/description)

## Question

Given a **0-indexed** string `word` and a character `ch`, reverse the segment of `word` that starts at index 0 and ends at the index of the first occurrence of `ch` (**inclusive**). If the character `ch` does not exist in `word`, do nothing.

For example, if `word  = "abcdefd"` and `ch = "d"`, then you should reverse the segment that starts at 0 and ends at 3 (inclusive). The resulting string will be `"dcbaefd"`.

Return the resulting string.

## Example 1

```text
Input: word = "abcdefd", ch = "d"
Output: "dcbaefd"
Explanation: The first occurrence of "d" is at index 3.
Reverse the part of word from 0 to 3 (inclusive), the resulting string is "dcbaefd".
```

## Example 2

```text
Input: word = "xyxzxe", ch = "z"
Output: "zxyxxe"
Explanation: The first and only occurrence of "z" is at index 3.
Reverse the part of word from 0 to 3 (inclusive), the resulting string is "zxyxxe".
```

## Example 3

```text
Input: word = "abcd", ch = "z"
Output: "abcd"
Explanation: "z" does not exist in word.
You should not do any reverse operation, the resulting string is "abcd".
```

## Constraints

- `1 <= word.length <= 250`
- `word` consists of lowercase English letters.
- `ch` is a lowercase English letter.

## Solution

```cpp
class Solution {
public:
    string reversePrefix(string word, char ch) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        int prefixIndex = INT_MIN;

        for(int i = 0;i<word.size();i++)
        {
            if(word[i]==ch)
            {
                prefixIndex = i;
                break;
            }
        }
        if(prefixIndex == INT_MIN)\
            return word;
        string ans = "";
        for(int i = prefixIndex;i>=0;i--)
            ans += word[i];

        for(int i = prefixIndex+1;i<word.size();i++)
            ans += word[i];

        return ans;
    }
};
```

## Complexity Analysis

- `Time` - O(N)
- `Space` - O(N)

## Explanation

1. Iterate through the string and check if the character `ch` exists in `word`.
2. If not return the `word` as it is.
3. If it exisits swap construct a new string `ans` in the following fashion.
4. Append characters from `prefixIndex` to `0`, then append characters from `prefixIndex +1` to `word.size()`.
5. Return the `ans` string.

### Note

- A solution using STL functions is as follows

```cpp
class Solution {
public:
    string reversePrefix(string word, char ch) {
        int j = word.find(ch);
        if (j != -1) {
            reverse(word.begin(), word.begin() + j + 1);
        }
        return word;
    }
};
```

---

> Showcases the string manipulation.
