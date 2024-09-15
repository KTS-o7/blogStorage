+++
title = 'Problem 3285 Find Indices of Stable Mountains'
date = 2024-09-15T16:08:07+05:30
draft = true
series = 'leetcode'
tags =['array','matrix','math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3285](https://leetcode.com/problems/find-indices-of-stable-mountains/)

## Question

There are `n` mountains in a row, and each mountain has a `height`. You are given an integer array `height` where `height[i]`represents the`height` of mountain `i`, and an integer `threshold`.

A mountain is called stable if the mountain just before it (if it exists) has a `height` strictly greater than `threshold`. Note that mountain `0` is not stable.

Return an array containing the indices of all stable mountains in any order.

## Example 1

```
Input: height = [1,2,3,4,5], threshold = 2

Output: [3,4]

Explanation:

Mountain 3 is stable because height[2] == 3 is greater than threshold == 2.
Mountain 4 is stable because height[3] == 4 is greater than threshold == 2.
```

## Example 2

```
Input: height = [10,1,10,1,10], threshold = 3

Output: [1,3]
```

## Example 3

```
Input: height = [10,1,10,1,10], threshold = 10

Output: []
```

## Constraints

- `2 <= n == height.length <= 100`
- `1 <= height[i] <= 100`
- `1 <= threshold <= 100`

## Solution

```cpp
class Solution {
public:
    vector<int> stableMountains(vector<int>& height, int threshold) {
        vector<int> result;
        for (int i = 1; i < height.size(); ++i) {
            if (height[i - 1] > threshold) {
                result.push_back(i);
            }
        }
        return result;
    }
};

```

## Complexity Analysis

```markdown
| Algorithm                      | Time Complexity | Space Complexity |
| ------------------------------ | --------------- | ---------------- |
| Array traversal and comparison | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition

The problem asks us to find the mountains whose previous mountain has a height greater than the threshold.

- Start from second mountain, compare the previous mountain's height with the threshold.
- If the previous mountain's height is greater than the threshold, add the index of the current mountain to the result.
- Else, continue to the next mountain.
- Return the result.

### 2. Implementation

```markdown
- Initialize an empty vector `result` to store the indices of stable mountains.
- Iterate over the array from `1` to `n-1`.
  - if `height[i-1]` is greater than `threshold`, add `i` to the `result`.
- Return the `result`.
```

> Biweekly Contest 139
