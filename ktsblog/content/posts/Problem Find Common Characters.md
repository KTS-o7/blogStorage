+++
title = 'Problem Find Common Characters'
date = 2024-06-06T23:34:53+05:30
draft = false
series = 'leetcode'
tags =['string','hash-table','vector','array']
toc = false
+++

# Problem Statement

**Link** - [Problem 1002](https://leetcode.com/problems/find-common-characters/description/)

## Question

Given a **string** array `words`, return an array of all characters that show up in all strings within the `words` (including duplicates). You may return the answer in any order.

## Example 1

```text
Input: words = ["bella","label","roller"]
Output: ["e","l","l"]
```

## Example 2

```text
Input: words = ["cool","lock","cook"]
Output: ["c","o"]
```

## Constraints

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` consists of lowercase English letters.

## Solution

```cpp
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        vector<int>baselineCount(26,0);
        string temp;
        for(int i =0;i<words[0].size();i++)
            {
                baselineCount[words[0][i]-'a']++;
            }
        for(auto word:words)
        {
            vector<int>newCount(26,0);
            for(const auto& ch:word)
                newCount[ch-'a']++;

            for(int i= 0;i<26;i++)
                baselineCount[i] = min(baselineCount[i],newCount[i]);
        }
        vector<string>answer;
        for(int i =0; i<26;i++)
        {
            while(baselineCount[i]--)
            {
                temp = "";
                temp += 'a'+i;
                answer.push_back(temp);
            }
        }
        return answer;


    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- We need to find the common characters in all the strings.
- We need some kind of hash table to store all the common characters.
- Characters may be repeated so we need to store the count of each character.
```

### 2. Implementation

```markdown
- Initialize a vector `baselineCount` of size 26 with all elements as `0`.
- Iterate over the first word and increment the count of each character in the `baselineCount`.
- Iterate over all the words and for each word create a new vector `newCount` of size 26 with all elements as `0`.
- Increment the count of each character in the `newCount`.
- Iterate over the `baselineCount` and `newCount` and update the `baselineCount` with the minimum of both.
- This will give us the count of each character that is common in all the words seen so far.
- Iterate over the `baselineCount` and for each character that has a count greater than `0` add it to the answer.
- Return the answer.
```

> This problem is about finding the common characters in a given set of strings.
