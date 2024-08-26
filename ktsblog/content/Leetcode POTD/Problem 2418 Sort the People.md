+++
title = 'Problem 2418 Sort the People'
date = 2024-07-22T18:47:32+05:30
draft = false
series = 'leetcode'
tags =['max-heap','sorting','arrays']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2418](https://leetcode.com/problems/sort-the-people/description/)

## Question

You are given an array of strings `names`, and an array `heights` that consists of distinct positive integers. Both arrays are of length `n`.

For each index `i`, `names[i]` and `heights[i]` denote the name and height of the `ith` person.

Return names sorted in **descending order** by the **people's heights**.

## Example 1

```
Input: names = ["Mary","John","Emma"], heights = [180,165,170]
Output: ["Mary","Emma","John"]
Explanation: Mary is the tallest, followed by Emma and John.
```

## Example 2

```
Input: names = ["Alice","Bob","Bob"], heights = [155,185,150]
Output: ["Bob","Alice","Bob"]
Explanation: The first Bob is the tallest, followed by Alice and the second Bob.
```

## Constraints

```markdown
- `n == names.length == heights.length`
- `1 <= n <= 10^3`
- `1 <= names[i].length <= 20`
- `1 <= heights[i] <= 10^5`
- `names[i] consists of lower and upper case English letters.`
- `All the values of heights are distinct`.
```

## Solution

```cpp
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        priority_queue<pair<int,string>,vector<pair<int,string>>>pq;
        for(int i=0;i<names.size();i++){
            pq.push(make_pair(heights[i],names[i]));
        }

        vector<string>ans;
        while(!pq.empty()){
            ans.push_back(pq.top().second);
            pq.pop();
        }
        return ans;

    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Heap      | O(nlogn)        | O(n)             |
```

## Explanation

### 1. Intuition

```markdown
- We need to sort the pairs of heights and names in descending order.
- Then append the names to the answer vector.
- We can use pair and priority queue to solve this problem.
```

### 2. Implementation

```markdown
- Initialize a priority queue of pairs of integers and strings named `pq`.
- Iterate over the names array and push the pair of heights and names to the priority queue.
- Initialize an empty vector of strings named `ans`.
- Iterate over the priority queue and append the names to the answer vector.
- Return the answer vector.
```
