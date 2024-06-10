+++
title = 'Problem Height Checker'
date = 2024-06-10T21:19:08+05:30
draft = false
series = 'leetcode'
tags =['vectors','sorting']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1051](https://leetcode.com/problems/height-checker/description/)

## Question

A school is trying to take an annual photo of all the students. The students are asked to stand in a single file line in non-decreasing order by height. Let this ordering be represented by the integer array `expected` where `expected[i]` is the expected height of the `ith` student in line.

You are given an integer array `heights` representing the current order that the students are standing in. Each `heights[i]` is the height of the `ith` student in line (0-indexed).

Return the number of indices where `heights[i] != expected[i]`.

## Example 1

```text
Input: heights = [1,1,4,2,1,3]
Output: 3
Explanation:
heights:  [1,1,4,2,1,3]
expected: [1,1,1,2,3,4]
Indices 2, 4, and 5 do not match.
```

## Example 2

```text
Input: heights = [5,1,2,3,4]
Output: 5
Explanation:
heights:  [5,1,2,3,4]
expected: [1,2,3,4,5]
All indices do not match.
```

## Example 3

```text
Input: heights = [1,2,3,4,5]
Output: 0
Explanation:
heights:  [1,2,3,4,5]
expected: [1,2,3,4,5]
All indices match.
```

## Constraints

- `1<= heights.length <= 100`
- `1 <= heights[i] <= 100`

## Solution

```cpp
class Solution {
public:
    int heightChecker(vector<int>& heights) {
        vector<int> ans;
        for(auto i : heights)
            ans.push_back(i);
        sort(ans.begin(), ans.end());
        int count = 0;
        for(int i = 0 ; i < heights.size() ; i ++)
            if(heights[i] != ans[i])
                count++;
        return count;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(NlogN)
- `Space Complexity` : O(N)

## Explanation

### 1. Intuition

```markdown
- We need to have a copy of the original array.
- Sort the copy.
- Compare the original array with the sorted array.
- Count the number of mismatches.
```

### 2. Implementation

```markdown
- Create a copy of the `heights` array and name it `expected`.
- Sort the `expected` array.
- Compare the `heights` array with the `expected` array.
- Count the number of mismatches.
- Return the count.
```
