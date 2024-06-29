+++
title = 'Problem All Ancestors of a Node in a DAG'
date = 2024-06-29T10:39:42+05:30
draft = false
series = 'leetcode'
tags =['graph','dfs','DAG','adjacency-list']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2192](https://leetcode.com/problems/all-ancestors-of-a-node-in-a-directed-acyclic-graph/description/)

## Question

You are given a positive integer `n` representing the number of nodes of a **Directed Acyclic Graph (DAG)**. The nodes are numbered from `0` to `n - 1` **(inclusive)**.

You are also given a 2D integer array `edges`, where `edges[i] = [fromi, toi]` denotes that there is a unidirectional edge from `fromi` to `toi` in the graph.

Return a list `answer`, where a`nswer[i]` is the li**st of ancestors of the `ith` node, sorted in ascending order**.

A node `u` is an ancestor of another node `v` if `u` can reach `v` via a set of edges.

## Example 1

```
Input: n = 8, edgeList = [[0,3],[0,4],[1,3],[2,4],[2,7],[3,5],[3,6],[3,7],[4,6]]
Output: [[],[],[],[0,1],[0,2],[0,1,3],[0,1,2,3,4],[0,1,2,3]]
Explanation:
The above diagram represents the input graph.
- Nodes 0, 1, and 2 do not have any ancestors.
- Node 3 has two ancestors 0 and 1.
- Node 4 has two ancestors 0 and 2.
- Node 5 has three ancestors 0, 1, and 3.
- Node 6 has five ancestors 0, 1, 2, 3, and 4.
- Node 7 has four ancestors 0, 1, 2, and 3.
```

{{<mermaid>}}
graph TD
0 --> 3
0 --> 4
1 --> 3
2 --> 4
2 --> 7
3 --> 5
3 --> 6
3 --> 7
4 --> 6
{{</mermaid>}}

## Example 2

```
Input: n = 5, edgeList = [[0,1],[0,2],[0,3],[0,4],[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Output: [[],[0],[0,1],[0,1,2],[0,1,2,3]]
Explanation:
The above diagram represents the input graph.
- Node 0 does not have any ancestor.
- Node 1 has one ancestor 0.
- Node 2 has two ancestors 0 and 1.
- Node 3 has three ancestors 0, 1, and 2.
- Node 4 has four ancestors 0, 1, 2, and 3.
```

{{<mermaid>}}
graph TD
0 --> 1
0 --> 2
0 --> 3
0 --> 4
1 --> 2
1 --> 3
1 --> 4
2 --> 3
2 --> 4
3 --> 4
{{</mermaid>}}

## Constraints

```markdown
- `1 <= n <= 1000`
- `0 <= edges.length <= min(2000, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `0 <= fromi, toi <= n - 1`
- `fromi != toi`
- There are no duplicate edges.
- The graph is **directed** and **acyclic**
```

## Solution

```cpp
class Solution {
public:
    void dfs(vector<bool>& visited, vector<vector<int>>& adjList, vector<vector<int>>& ancestor ,int parent, int curr)
    {
        visited[curr] = true;
        for( int forward:adjList[curr])
        {
            if(!visited[forward])
            {
                ancestor[forward].push_back(parent);
                dfs(visited,adjList,ancestor,parent,forward);
            }
        }
    }

    vector<vector<int>> getAncestors(int n, vector<vector<int>>& edges) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        vector<vector<int>> adjList(n);
        vector<vector<int>> ancestor(n);

        for (const auto& edge : edges) {
            adjList[edge[0]].push_back(edge[1]);
        }


        for(int i = 0; i<n; i++){
            vector<bool>visited(n,false);
            dfs(visited,adjList,ancestor,i,i);
        }

        //for(int i = 0;i <n; i++)
            //sort(ancestor[i].begin(),ancestor[i].end());

        return ancestor;

    }
};
```

## Complexity Analysis

- `Time Complexity` : O(n) - where `n` is the number of nodes in the graph
- `Space Complexity` : O(n) - where `n` is the number of nodes in the graph

## Explanation

### 1. Intuition

```markdown
- We need to find the ancestors of each node in the graph and store it in sorted order in form of a list.
- To find the ancestors of a node we can perform a DFS traversal of the graph.
- We can start the DFS traversal from each node and keep track of the parent node.
- For each children nodes which is reachable from the current node, we can add the parent node as an ancestor.
- To prevent multiple visits to the same node we can use a visited array.
```

### 2. Implementation

```markdown
- We can create an adjacency list `adjList` to store the graph.
- We can create a 2D vector `ancestor` to store the ancestors of each node.
- Now we will perform the DFS traversal of the graph.
- For each node from `0` to `n-1` , create a new `visted` array and call the `dfs` function.

- DFS Function
  - Input : `visited` array, `adjList`, `ancestor` array, `parent` node, `current` node
  - Output : void
  - mark the current node as visited
  - for each forward node reachable from the current node
    - if the forward node is not visited
      - add the parent node as an ancestor of the forward node
      - call the dfs function recursively for the forward node
```

### 3. Dry Run

```markdown
- Let's dry run the first example to understand the solution better.
- We have `n = 8` and `edgeList = [[0,3],[0,4],[1,3],[2,4],[2,7],[3,5],[3,6],[3,7],[4,6]]`
- The adjaceny list is as follows:
  - 0 -> 3,4
  - 1 -> 3
  - 2 -> 4,7
  - 3 -> 5,6,7
  - 4 -> 6
  - 5 ->
  - 6 ->
  - 7 ->
- We will start the DFS traversal from each node and find the ancestors.
- For node 0, the ancestors are []
- For node 1, the ancestors are []
- For node 2, the ancestors are []
- For node 3, the ancestors are [0,1]
- For node 4, the ancestors are [0,2]
- For node 5, the ancestors are [0,1,3]
- For node 6, the ancestors are [0,1,2,3,4]
- For node 7, the ancestors are [0,1,2,3]
```

> The solution automatically gives the output in sorted form because the order of parent nodes is maintained while performing the DFS traversal.
