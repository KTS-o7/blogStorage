+++
title = 'Problem 1971 Find if Path Exists in Graph'
date = 2024-04-21T15:42:16+05:30
draft = false
series = 'leetcode'
tags = ['cpp','graph','dfs']
toc = false
+++

# Problem Statement

**Link** - [Problem 1971](https://leetcode.com/problems/find-if-path-exists-in-graph/description/)

## Question

There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n-1` **(inclusive)**. The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [ui, vi]` denotes a **bi-directional** edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

You want to determine if there is a valid path that exists from vertex `source` to vertex `destination`.

Given `edges` and the integers `n`, `source`, and `destination`, return `true` if there is a valid path from source to destination, or `false` otherwise.

## Example 1

{{<mermaid>}}
graph LR
A((0))<-->B((1))
B((1))<-->C((2))
C((2))<-->A((0))
{{</mermaid>}}

```text
Input: n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
Output: true
Explanation: There are two paths from vertex 0 to vertex 2:
- 0 → 1 → 2
- 0 → 2
```

## Example 2

{{<mermaid>}}
graph TD
A((0))<-->B((1))
C((2))<-->A((0))
D((3))<-->E((4))
E((4))<-->F((5))
F((5))<-->D((3))
{{</mermaid>}}

```text
Input: n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
Output: false
Explanation: There is no path from vertex 0 to vertex 5.
```

## Edge Case

{{<mermaid>}}
graph TD
A((0))
{{</mermaid>}}

```text
Input: n = 1, edges = [], source = 0,
destination = 0
Output = true
Explanation: We are already in destination node.
```

## Constraints

- `1 <= n <= 2 * 10^5`
- `0 <= edges.length <= 2 * 10^5`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- There are no duplicate edges.
- There are no self edges

## Solution

```cpp
class Solution {
public:
    bool dfs(unordered_map<int,vector<int>>& graph, int curr, int dest, unordered_set<int>& visited)
    {
        if(curr == dest)
            return true;

        visited.insert(curr);
        for(int next:graph[curr])
        {
            if(visited.find(next) == visited.end())
                if( dfs(graph,next,dest,visited))
                    return true;
        }
        return false;
    }
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        unordered_map<int, vector<int>> graph;
        for (const auto& edge : edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }

        unordered_set<int> visited;
        return dfs(graph, source, destination, visited);
    }
};
```

## Complexity

- `Time : O(V + E)`, DFS visits each node and checks each edge atleast once.
- `Space : O(V + E)`, the map stores each vertex `V` and corresponding edges `E`.

## Explanation

1. We are given a vector of vector of `int` which denote the edges of a graph, the source vertex and destination vertex.
2. Graph is bidirectional and there are no Self-Edges (From given).
3. First we create an adjacency list `graph` which stores the vertex and the edges which is connected to them.
4. We keep track of visited nodes in an unordered set `visited`.
5. The DFS function then first checks if the current node is destination.
6. If yes then we return `true`.
7. Else we will append it to `visited` set, then try to find if any path exists from the connected nodes of current node.
8. We will explore all the unvisited conntected nodes.
9. If no path is found we return `false`.

---

> This shows the dfs method of graph traversal when a graph is represented in adjacency list format.
