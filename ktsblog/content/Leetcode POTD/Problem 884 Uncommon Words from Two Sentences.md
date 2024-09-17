+++
title = 'Problem 884 Uncommon Words From Two Sentences'
date = 2024-09-17T12:53:11+05:30
draft = true
series = 'leetcode'
tags =['hash-map','string','count']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 884](https://leetcode.com/problems/uncommon-words-from-two-sentences/description/)

## Question

A sentence is a string of single-space separated words where each word consists only of lowercase letters.

A word is uncommon if it appears exactly once in one of the sentences, and does not appear in the other sentence.

Given two sentences `s1` and `s2`, return a list of all the uncommon words. You may return the answer in any order.

## Example 1

```
Input: s1 = "this apple is sweet", s2 = "this apple is sour"

Output: ["sweet","sour"]

Explanation:

The word "sweet" appears only in s1, while the word "sour" appears only in s2.
```

## Example 2

```
Input: s1 = "apple apple", s2 = "banana"

Output: ["banana"]
```

## Constraints

- 1 <= `s1.length`, `s2.length` <= 200
- `s1` and `s2` consist of lowercase English letters and spaces.
- `s1` and `s2` do not have leading or trailing spaces.
- All the words in `s1` and `s2` are separated by a single space.

## Solution

```cpp
class Solution {
public:
    void wordsplitcount(string s,unordered_map<string,int>& mp)
    {
        stringstream ss(s);
        string word;
        while(ss>>word)
            mp[word]++;
    }

    vector<string> uncommonFromSentences(string s1, string s2)
    {
        unordered_map<string,int>mp;
        wordsplitcount(s1,mp);
        wordsplitcount(s2,mp);

        vector<string>ans;
        for(auto x:mp)
            if(x.second==1)
                ans.push_back(x.first);

        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm  | Time Complexity | Space Complexity |
| ---------- | --------------- | ---------------- |
| countWords | O(N)            | O(N)             |
```

## Explanation

### 1. Intuition

- This question is a counting problem. We need to satisfy two conditions to get the uncommon words.
  1. The word should appear only once in the sentence.
  2. The word should not appear in the other sentence.
- This means that the frequency of the word should be 1 in the combined sentence.
- We can use a hash map to store the frequency of each word in the combined sentence.
- We can then iterate over the hash map and check if the frequency of the word is 1.
- If the frequency is 1, we can add the word to the answer vector.
- Finally, we can return the answer vector.

### 2. Implementation

```markdown
- We can create a helper function `wordsplitcount` that takes a string and a hash map as input.
- We can use a string stream to split the string into words.
- We can then iterate over the words and increment the frequency of each word in the hash map.
- We can create a hash map `mp` to store the frequency of each word in the combined sentence.
- We can call the `wordsplitcount` function for both sentences to populate the hash map.
- We can create an answer vector `ans` to store the uncommon words.
- We can iterate over the hash map and check if the frequency of the word is 1.
- If the frequency is 1, we can add the word to the answer vector.
- Finally, we can return the answer vector.
```
