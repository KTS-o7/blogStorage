+++
title = 'Problem 1684 Count the Number of Consistent Strings'
date = 2024-09-12T11:38:56+05:30
draft = false
series = 'leetcode'
tags =['string','array','hash-map','hash-set','hash-map','count']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1684](https://leetcode.com/problems/count-the-number-of-consistent-strings/description/)

## Question

You are given a string `allowed` consisting of distinct characters and an array of strings `words`. A string is consistent if all characters in the string appear in the string `allowed`.

Return the number of consistent strings in the array `words`.

## Example 1

```
Input: allowed = "ab", words = ["ad","bd","aaab","baa","badab"]
Output: 2
Explanation: Strings "aaab" and "baa" are consistent since they only contain characters 'a' and 'b'.
```

## Example 2

```
Input: allowed = "abc", words = ["a","b","c","ab","ac","bc","abc"]
Output: 7
Explanation: All strings are consistent.
```

## Example 3

```
Input: allowed = "cad", words = ["cc","acd","b","ba","bac","bad","ac","d"]
Output: 4
Explanation: Strings "cc", "acd", "ac", and "d" are consistent.
```

## Constraints

- 1 <= `words.length` <= 10<sup>4</sup>
- 1 <= `allowed.length` <= 26
- 1 <= `words[i].length` <= 10
- The characters in `allowed` are distinct.
- `words[i]` and `allowed` contain only lowercase English letters.

## Solution

```cpp
class Solution {
public:
    int countConsistentStrings(string allowed, vector<string>& words) {
        std::ios::sync_with_stdio(false);
        vector<int>s(26,0);
        for(int i =0; i<allowed.size(); i++){
            s[allowed[i]-'a']++;
        }
        int count = 0;
        bool flag = true;
        for(const auto& word:words){
            flag = true;
            for(int i = 0; i<word.size(); i++){
                if(s[word[i]-'a'] == 0){
                    flag = false;
                    break;
                }
            }
            if(flag)
                count++;
        }
        return count;

    }
};
```

## Complexity Analysis

```markdown
| Algorithm       | Time Complexity | Space Complexity |
| --------------- | --------------- | ---------------- |
| frequency array | O(n)            | O(1)             |
| Counting        | O(nm)           | O(1)             |
```

## Explanation

### 1. Intuition

Lets try to understand what the problem is asking.

- We are given a string `allowed` and an array of strings `words`.
- Count the number of strings in the array `words` which are made up of characters present in the string `allowed`.
- So we need a way to check if a character from the string `words` is present in the string `allowed`.
- So we can use a frequency array to store the frequency of characters in the string `allowed`.
- Then we can iterate over the array `words` and check if all the characters in the string `words` are present in the string `allowed`.
- If all the characters are present then we can increment the count.

### 2. Implementation

```markdown
- Initialize a frequency array `s` of size 26 with all elements as 0.
- Iterate over the string `allowed` and increment the frequency of the character in the frequency array.
- Initialize a variable `count` to store the count of consistent strings.
- Initialize a boolean variable `flag` to check if the string is consistent.
- Iterate over the array `words` and for each word check if all the characters are present in the string `allowed`.
  - If any character is not present then set the flag to false and break.
- If the flag is true then increment the count.
- Return the count.
```
