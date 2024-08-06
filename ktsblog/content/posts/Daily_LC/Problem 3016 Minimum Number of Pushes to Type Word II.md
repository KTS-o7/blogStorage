+++
title = 'Problem 3016 Minimum Number of Pushes to Type Word II'
date = 2024-08-06T20:59:08+05:30
draft = false
series = 'leetcode'
tags =['string','greedy','sorting']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3016](https://leetcode.com/problems/minimum-number-of-pushes-to-type-word-ii/description/)

## Question

You are given a string `word` containing lowercase English letters.

Telephone keypads have keys mapped with distinct collections of lowercase English letters, which can be used to form words by pushing them. For example, the key `2` is mapped with `["a","b","c"]`, we need to push the key one time to type `"a"`, two times to type `"b"`, and three times to type `"c"` .

It is allowed to remap the keys numbered `2` to `9` to distinct collections of letters. The keys can be remapped to any amount of letters, but each letter must be mapped to exactly one key. You need to find the minimum number of times the keys will be pushed to type the string `word`.

Return the minimum number of pushes needed to type `word` after remapping the keys.

An example mapping of letters to keys on a telephone keypad is given below. Note that `1`, `*`, `#`, and `0` do not map to any letters.

## Example 1

```
Input: word = "abcde"
Output: 5
Explanation: The remapped keypad given in the image provides the minimum cost.
"a" -> one push on key 2
"b" -> one push on key 3
"c" -> one push on key 4
"d" -> one push on key 5
"e" -> one push on key 6
Total cost is 1 + 1 + 1 + 1 + 1 = 5.
It can be shown that no other mapping can provide a lower cost.
```

## Example 2

```
Input: word = "xyzxyzxyzxyz"
Output: 12
Explanation: The remapped keypad given in the image provides the minimum cost.
"x" -> one push on key 2
"y" -> one push on key 3
"z" -> one push on key 4
Total cost is 1 * 4 + 1 * 4 + 1 * 4 = 12
It can be shown that no other mapping can provide a lower cost.
Note that the key 9 is not mapped to any letter
it is not necessary to map letters to every key, but to map all the letters.
```

## Example 3

```
Input: word = "aabbccddeeffgghhiiiiii"
Output: 24
Explanation: The remapped keypad given in the image provides the minimum cost.
"a" -> one push on key 2
"b" -> one push on key 3
"c" -> one push on key 4
"d" -> one push on key 5
"e" -> one push on key 6
"f" -> one push on key 7
"g" -> one push on key 8
"h" -> two pushes on key 9
"i" -> one push on key 9
Total cost is 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 2 * 2 + 6 * 1 = 24.
It can be shown that no other mapping can provide a lower cost.
```

## Constraints

```markdown
- `1 <= word.length <= 10^5`
- `word` consists of lowercase English letters.
```

## Solution

```cpp
class Solution {
public:
    int minimumPushes(string word) {
        vector<int> v(26);
        for (char& c : word)
            v[c - 'a']++;
        sort(v.begin(), v.end(), greater<int>());
        int a = 0;
        for (int i = 0; i < 26; ++i)
            a += v[i] * (i / 8 + 1);
        return a;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Sorting   | O(26log26)      | O(26)            |
| Counting  | O(n)            | O(26)            |
```

## Explanation

### 1. Intuition

```markdown
- The minimum number of pushes to type a word will occur when the most frequent letter is mapped to the key with the least number of pushes.
- Hence we need to keep track of the frequency of each letter and sort them in descending order.
- Then we can iterate over the sorted array and calculate the minimum number of pushes required to type the word.
```

### 2. Implementation

```markdown
- Initialize a vector `v` of size `26` to keep track of the frequency of each letter.
- Iterate over the word and increment the frequency of each letter.
- Sort the vector in descending order.
- Initialize a variable `a` to keep track of the minimum number of pushes required.
- Iterate over the sorted vector and calculate the minimum number of pushes required to type the word
  - `a += v[i] * (i / 8 + 1)`
  - here `i / 8 + 1` calculates the number of pushes required to type the letter.
- Return the minimum number of pushes required.
```

