+++
title = 'Problem Subsets'
date = 2024-05-21T21:13:39+05:30
draft = false
series = 'leetcode'
tags =['backtracking','recursion','iterative','cpp']
toc = false
+++

# Problem Statement

**Link** - [Problem 78](https://leetcode.com/problems/subsets/description/)

## Question

Given an integer array `nums` of unique elements, return all possible **subsets** (the power set).

- A subset of an array is a selection of elements (possibly none) of the array.

The solution set must not contain duplicate subsets. Return the solution in any order.

## Example 1

```text
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

## Example 2

```text
Input: nums = [0]
Output: [[],[0]]
```

## Constraints

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of nums are **unique**.

## Solution

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;
        result.push_back({});
        int n;
        for (int num : nums) {

             n = result.size();

            for (int i = 0; i < n; ++i) {
                vector<int> subset = result[i];
                subset.push_back(num);
                result.push_back(subset);
            }
        }

        return result;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N \* 2^N) - 2^N subsets and each subset has N elements
- `Space Complexity` : O(N \* 2^N) - 2^N subsets and each subset has N elements

## Explanation

### 1. Intuition

- The idea is to start with an empty subset and keep adding elements to it.
- For each element in the input array, we add it to all the existing subsets and create new subsets.

### 2. Implementation

- We start with an empty subset and add it to the `result`.
- For each element in the input array, we iterate over all the existing subsets and create new subsets by adding the current element to them.
- We add these new subsets to the `result`.
- Finally, we return the `result` containing all the subsets.

## Solution with backtracking

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;
        vector<int> subset;
        backtrack(result, subset, nums, 0);
        return result;
    }

    void backtrack(vector<vector<int>>& result, vector<int>& subset, vector<int>& nums, int start) {
        result.push_back(subset);
        for (int i = start; i < nums.size(); ++i) {
            subset.push_back(nums[i]);
            backtrack(result, subset, nums, i + 1);
            subset.pop_back();
        }
    }
};
```

## Complexity Analysis of backtracking solution

- `Time Complexity` : O(N \* 2^N) - 2^N subsets and each subset has N elements
- `Space Complexity` : O(N) - The depth of the recursion tree can go up to N

## Explanation of backtracking solution

### 1. Intuition

- We can either add an element to the subset or not add it.
- We will get one subset with element added to it and another subset without the element.
- Recursion tree for the input [1,2,3]
  {{<mermaid>}}
  graph TD
  A[ ] --> B1[1]
  A[ ] --> C[ ]
  B1[1] --> D12[1, 2]
  B1[1] --> E1[1]
  D12[1, 2] --> F123[1, 2, 3]
  D12[1, 2] --> G12[1, 2]
  E1[1] --> L13[1, 3]
  E1[1] --> M1[1]
  C[ ] --> R2[2]
  C[ ] --> S[ ]
  R2[2] --> T23[2, 3]
  S[ ] --> Z3[3]
  {{</mermaid>}}

### 2. Implementation

- `result` is the vector of all subsets.
- For each element in the input array, we add it to the subset and call the `backtrack` function recursively.
- In the `backtrack` function, we add the subset to the `result` and iterate over the remaining elements in the input array.
- This is ensured by starting the loop from the `start` index.
- Then we remove the current element from the subset once the recursive call returns.

---

> Shows the usage of backtracking to ensure proper subset generation.
