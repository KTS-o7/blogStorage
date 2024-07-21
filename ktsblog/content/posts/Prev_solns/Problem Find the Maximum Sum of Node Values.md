+++
title = 'Problem 3068 Find the Maximum Sum of Node Values'
date = 2024-05-19T10:30:05+05:30
draft = false
series = 'leetcode'
tags =['XOR','bit-manipulation','tree','greedy']
toc = false
+++

# Problem Statement

**Link** - [Problem 3068](https://leetcode.com/problems/find-the-maximum-sum-of-node-values/description/)

## Question

There exists an **undirected tree** with `n` nodes numbered `0` to `n - 1`. You are given a **0-indexed** 2D integer array `edges` of length n - 1, where `edges[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the tree. You are also given a positive integer `k`, and a 0-indexed array of non-negative integers `nums` of length n, where `nums[i]` represents the value of the node numbered i.

Alice wants the sum of values of tree nodes to be maximum, for which Alice can perform the following operation any number of times (including zero) on the tree:

Choose any edge `[u, v]` connecting the nodes `u` and `v`, and update their values as follows:

- n`ums[u] = nums[u] XOR k`
- `nums[v] = nums[v] XOR k`
  Return the **maximum** possible **sum** of the **values** Alice can achieve by performing the operation any number of times.

## Example 1

```text
Input: nums = [1,2,1], k = 3, edges = [[0,1],[0,2]]
Output: 6
Explanation: Alice can achieve the maximum sum of 6 using a single operation:
- Choose the edge [0,2]. nums[0] and nums[2] become: 1 XOR 3 = 2, and the array nums becomes: [1,2,1] -> [2,2,2].
The total sum of values is 2 + 2 + 2 = 6.
It can be shown that 6 is the maximum achievable sum of values.
```

{{<mermaid>}}
flowchart TD
id1(input)
A((1)) --> B((2))
A --> C((1))
{{</mermaid>}}

## Example 2

```text
Input: nums = [2,3], k = 7, edges = [[0,1]]
Output: 9
Explanation: Alice can achieve the maximum sum of 9 using a single operation:
- Choose the edge [0,1]. nums[0] becomes: 2 XOR 7 = 5 and nums[1] become: 3 XOR 7 = 4, and the array nums becomes: [2,3] -> [5,4].
The total sum of values is 5 + 4 = 9.
It can be shown that 9 is the maximum achievable sum of values.
```

{{<mermaid>}}
flowchart TD
id1(input)
A((2)) --> B((3))
{{</mermaid>}}

## Example 3

```text
Input: nums = [7,7,7,7,7,7], k = 3, edges = [[0,1],[0,2],[0,3],[0,4],[0,5]]
Output: 42
Explanation: The maximum achievable sum is 42 which can be achieved by Alice performing no operations.
```

{{<mermaid>}}
flowchart TD
id1(input)
A((7)) --> B((7))
A --> C((7))
A --> D((7))
A --> E((7))
A --> F((7))
{{</mermaid>}}

## Constraints

- `2 <= n == nums.length <= 2 * 10*4`
- `1 <= k <= 10^9`
- `0 <= nums[i] <= 10^9`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= edges[i][0], edges[i][1] <= n - 1`
- The input is generated such that edges represent a valid tree.

## Solution

```cpp
static auto fastio = [](){
            std::ios::sync_with_stdio(false);
            cin.tie(nullptr);
            cout.tie(nullptr);
            return nullptr;
        };

class Solution {
public:
    long long maximumValueSum(vector<int>& nums, int k, vector<vector<int>>& edges) {
        long long res = 0;
        int changes = 0;
        int minDiff = INT_MAX;

        for (int num : nums) {
            int xorNum = num ^ k;

            if (num < xorNum) {
                changes++;
                res += xorNum;
            } else {
                res += num;
            }

            minDiff = min(minDiff, abs(num - xorNum));
        }

        if (changes % 2 == 1)
            return res - minDiff;
        else
            return res;
    }

};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

- The problem is to find the maximum sum of the values of the nodes in the tree.
- We know that since its a tree, a path can be found between any two nodes.
- Hence we need to make sure that only even number of nodes need to be updated.

### 2. Implementation

- We iterate over the nodes and calculate the XOR value of the node with `k`.
- If the XOR value is greater than the original value, we update the node and increment the changes.
- We also calculate the minimum difference between the original value and the XOR value.
- If the number of changes is odd, we subtract the minimum difference from the result.
- This step is done to make sure that only even number of nodes are updated.
- Finally, we return the result.

---

> There are DP solutions possible for this problem, but the above greedy solution is the most optimal one. -> Click [DP Solution In Leetcode](https://leetcode.com/problems/find-the-maximum-sum-of-node-values/editorial/)
