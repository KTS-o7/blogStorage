+++
title = 'Problem 3264 Final Array State After K Multiplication Operations I'
date = 2024-08-25T12:18:01+05:30
draft = false
series = 'leetcode'
tags =['priority-queue','min-heap','math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3264](https://leetcode.com/problems/final-array-state-after-k-multiplication-operations-i/description/)

## Question

You are given an integer array `nums`, an integer `k`, and an integer `multiplier`.

You need to perform `k` operations on nums. In each operation:

- Find the minimum value `x` in `nums`. If there are multiple occurrences of the minimum value, select the one that appears first.
- Replace the selected minimum value `x` with `x * multiplier`.

Return an integer array denoting the final state of `nums` after performing all `k` operations.

## Example 1

```
Input: nums = [1,2], k = 3, multiplier = 4

Output: [16,8]

Explanation:

| Operation         | Result  |
| ----------------- | ------- |
| After operation 1 | [4, 2]  |
| After operation 2 | [4, 8]  |
| After operation 3 | [16, 8] |
```

## Example 2

```
Input: nums = [2,1,3,5,6], k = 5, multiplier = 2

Output: [8,4,6,5,6]

| Operation         | Array           |
| ----------------- | --------------- |
| After operation 1 | [2, 2, 3, 5, 6] |
| After operation 2 | [4, 2, 3, 5, 6] |
| After operation 3 | [4, 4, 3, 5, 6] |
| After operation 4 | [4, 4, 6, 5, 6] |
| After operation 5 | [8, 4, 6, 5, 6] |
```

## Constraints

```markdown
- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `1 <= k <= 10`
- `1 <= multiplier <= 5`
```

## Solution

```cpp
// Brute Force
class Solution {
public:
    vector<int> getFinalState(vector<int>& nums, int k, int multiplier) {
        for (int i = 0; i < k; i++) {
            int minIndex = 0;
            for (int j = 1; j < nums.size(); j++) {
                if (nums[j] < nums[minIndex]) {
                    minIndex = j;
                }
            }
            nums[minIndex] *= multiplier;
        }
        return nums;
    }
};

// Min Heap

class Solution {
public:
    vector<int> getFinalState(vector<int>& nums, int k, int multiplier) {
        int n = nums.size();
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

        for (int i = 0; i < n; ++i) {
            pq.push({nums[i], i});
        }

        while (k > 0) {
            auto [val, idx] = pq.top();
            pq.pop();
            val = (val * multiplier);
            pq.push({val, idx});
            k--;
        }
        vector<int> result(n);
        while (!pq.empty()) {
            auto [val, idx] = pq.top();
            pq.pop();
            result[idx] = val;
        }

        return result;
    }
};

```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Brute     | O(kn)           | O(1)             |
| Min Heap  | O(nlogn)        | O(n)             |
```

## Explanation

### 1. Intuition

```markdown
- In brute force we need to find the minimum element in the array
  then multiply it with the multiplier.
- First traverse the entire array to find the index of the minimum element.
- Then multiply the element at that index with the multiplier.

- In the min heap approach, we can use a min heap to store elements and their indices.
- We can pop the top element from the heap and multiply it with the multiplier.
- Then push the new element back into the heap.
- Repeat this process for k times.
- Finally, we can construct the final array from the heap.
```

### 2. Implementation

```markdown
- For the brute force approach

  - Iterate over the array for k times.
    - Find the index of the minimum element in the array.
    - Multiply the element at that index with the multiplier.
  - Return the final array.

- For the min heap approach
  - Initialize a min heap `pq` of type `pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>`.
    - Iterate over the array and push the elements along with their indices to the heap.
    - Iterate from `0` to `k`:
      - Get the top element from the heap and store it in `val` and `idx`.
      - Multiply the `val` with the multiplier.
      - Push the new element back into the heap.
    - Initialize a vector `result` of size `n`.
    - Iterate over the heap and store the elements in the `result`.
    - Return the `result`.
```

> There is a follow up question for this problem, [Problem 3265](https://leetcode.com/problems/final-array-state-after-k-multiplication-operations-ii/description/).
