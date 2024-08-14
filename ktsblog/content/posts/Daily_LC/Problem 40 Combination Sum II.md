+++
title = 'Problem 40 Combination Sum II'
date = 2024-08-13T10:33:14+05:30
draft = false
series = 'leetcode'
tags =['array','backtracking']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 40](https://leetcode.com/problems/combination-sum-ii/description/)

## Question

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to target.

Each number in `candidates` may only be used once in the combination.

**Note:** The solution set must not contain duplicate combinations.

## Example 1

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

## Example 2

```
Input: candidates = [2,5,2,1,2], target = 5
Output:
[
[1,2,2],
[5]
]
```

## Constraints

```markdown
- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`
```

## Solution

```cpp
/class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<vector<int>> result;
        vector<int> current;
        backtrack(candidates, target, 0, current, result);
        return result;
    }

private:
    void backtrack(vector<int>& candidates, int target, int start, vector<int>& current, vector<vector<int>>& result) {
        if (target == 0) {
            result.push_back(current);
            return;
        }
        for (int i = start; i < candidates.size(); i++) {
            if (i > start && candidates[i] == candidates[i - 1])
                continue;
            if (candidates[i] > target)
                break;
            current.push_back(candidates[i]);
            backtrack(candidates, target - candidates[i], i + 1, current, result);
            current.pop_back();
        }
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Backtrack | O(2^N)          | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- To generate all combinations, we can use backtracking.
- But we need to avoid duplicates. Hence we sort the array and skip the duplicates.
- `Backtrack` function is called recursively to generate all possible combinations.
- In backtracking, we add the current element to the `current` vector and call the backtrack function recursively.
- If it reaches the target, we add the current vector to the result.
- If the current element is greater than the target, we break the loop.
- If the current element is a duplicate, we skip it.
- We pop the last element from the current vector after the recursive call.
```

### 2. Implementation

```markdown
- Sort the array to avoid duplicates.
- Initialize the `result` vector to store the final result.
- Initialize the `current` vector to store the current combination.
- Call the `backtrack` function with the `candidates`, `target`, `start`, `current`, and `result`.
- In the `backtrack` function, if the target is 0, add the current vector to the result.
- Iterate over the candidates array from the `start` index.
- If the current element is a duplicate, skip it.
- If the current element is greater than the target, break the loop.
- Add the current element to the current vector and call the backtrack function recursively.
- Pop the last element from the current vector after the recursive call.
```
