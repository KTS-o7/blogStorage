+++
title = 'Problem 2053 Kth Distinct String in an Array'
date = 2024-08-05T13:09:46+05:30
draft = false
series = 'leetcode'
tags =['hashmap','string',]
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2053](https://leetcode.com/problems/kth-distinct-string-in-an-array/description/)

## Question

A distinct string is a string that is present only once in an array.

Given an array of strings `arr`, and an integer `k`, return the `kth` distinct string present in `arr`. If there are fewer than `k` distinct strings, return an empty string `""`.

Note that the strings are considered in the order in which they appear in the array.

## Example 1

```
Input: arr = ["d","b","c","b","c","a"], k = 2
Output: "a"
Explanation:
The only distinct strings in arr are "d" and "a".
"d" appears 1st, so it is the 1st distinct string.
"a" appears 2nd, so it is the 2nd distinct string.
Since k == 2, "a" is returned.
```

## Example 2

```
Input: arr = ["aaa","aa","a"], k = 1
Output: "aaa"
Explanation:
All strings in arr are distinct, so the 1st string "aaa" is returned.
```

## Example 3

```
Input: arr = ["a","b","a"], k = 3
Output: ""
Explanation:
The only distinct string is "b". Since there are fewer than 3 distinct strings, we return an empty string "".
```

## Constraints

```markdown
- `1 <= k <= arr.length <= 1000`
- `1 <= arr[i].length <= 5`
- `arr[i]` consists of lowercase English letters.
```

## Solution

```cpp
class Solution {
public:
    string kthDistinct(vector<string>& arr, int k) {
        std::ios::sync_with_stdio(false);
        unordered_map<string,int>freq;
        for(auto& it:arr){
            freq[it]++;
        }
        for(auto& s : arr) {
            if(freq[s] == 1 && --k == 0)
                return s;
        }

        return "";
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Hashmap   | O(N)            | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- this problem requires us to keep track of the frequency of each string in the array.
- Frequency of distinct string is `1`.
- Then we can iterate over the array and check if the frequency of the string is `1` and value of `k` is `0` 
then return the string.
- If we reach the end of the array and still not found the `kth` distinct string then return an empty string.
```

### 2. Implementation

```markdown
- Create a hashmap to store the frequency of each string.
- Iterate over the array and store the frequency of each string.
- Iterate over the array and check if the frequency of the string is `1` and value of `k` is `0`
  then return the string.
- Has to decrement the value of k
  when we find the distinct string.
- If we reach the end of the array and still not found the `kth` distinct string
  then return an empty string.
```
