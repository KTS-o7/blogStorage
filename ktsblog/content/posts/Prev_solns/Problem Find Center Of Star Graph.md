+++
title = 'Problem 1791 Find Center of Star Graph'
date = 2024-06-27T10:59:29+05:30
draft = false
series = 'leetcode'
tags =['graph','star-graph','degree']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1791](https://leetcode.com/problems/find-center-of-star-graph/description/)

## Question

There is an undirected star graph consisting of `n` nodes labeled from `1` to `n`. A star graph is a graph where there is one center node and exactly `n - 1` edges that connect the center node with every other node.

You are given a 2D integer array `edges` where each `edges[i] = [ui, vi]` indicates that there is an edge between the nodes `ui` and `vi`. Return the center of the given star graph.

## Example 1

```text
Input: edges = [[1,2],[2,3],[4,2]]
Output: 2
Explanation: As shown in the figure above, node 2 is connected to every other node, so 2 is the center.
```

{{<mermaid>}}
graph TD
A((1)) --- B((2))
B --- C((3))
D((4)) --- B
{{</mermaid>}}

## Example 2

```text
Input: edges = [[1,2],[5,1],[1,3],[1,4]]
Output: 1
```

{{<mermaid>}}
graph TD
A((2)) --- B((1))
A --- C((3))
A --- D((4))
E((5)) --- A
{{</mermaid>}}

## Constraints

```markdown
- `3 <= n <= 10^5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- The given `edges` represent a valid star graph.
```

## Solution

```cpp
class Solution {
public:
    int findCenter(vector<vector<int>>& edges) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        return edges[0][0]==edges[1][0] || edges[0][0] == edges[1][1]?edges[0][0]:edges[0][1];
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(1)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- Since it's a star graph it means that the center node will have the maximum degree.
- `Also none of the other nodes will have degree more than 1.`
- So we can just check the first two edges and return the node which is common in both the edges.
```

### 2. Implementation

```markdown
- We can just check the first two edges and return the node which is common in both the edges.
```

> Simple graph question which teaches the characteristics of a star graph.
