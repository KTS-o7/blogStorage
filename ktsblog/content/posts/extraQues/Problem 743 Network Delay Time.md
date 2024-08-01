+++
title = 'Problem 743 Network Delay Time'
date = 2024-08-01T18:41:19+05:30
draft = false
series = 'leetcode'
tags =['dijkstras','graph','shortest-path']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 743](https://leetcode.com/problems/network-delay-time/description/)

## Question

You are given a network of `n` nodes, labeled from `1` to `n`. You are also given times, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return the minimum time it takes for all the `n` nodes to receive the signal. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

## Example 1

![image1](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

```
Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2
```

## Example 2

```
Input: times = [[1,2,1]], n = 2, k = 1
Output: 1
```

## Example 3

```
Input: times = [[1,2,1]], n = 2, k = 2
Output: -1
```

## Constraints

```markdown
- `1 <= k <= n <= 100`
- `1 <= times.length <= 6000`
- `times[i].length == 3`
- `1 <= ui, vi <= n`
- `ui != vi`
- `0 <= wi <= 100`
- All the pairs `(ui, vi)` are unique. (i.e., no multiple edges.)
```

## Solution

```cpp
class Solution {
public:

    vector<int> dijkstras(int src,vector<vector<pair<int,int>>>&adj,int V){
        vector<int>dist(V+1,INT_MAX);
        dist[src] = 0;
        priority_queue<pair<int,int>,vector<pair<int,int>>, greater<pair<int,int>>>pq;
        pq.push({0,src});
        while(!pq.empty()){
            int node = pq.top().second;
            int wt = pq.top().first;
            pq.pop();
            for(auto it:adj[node]){
                if(wt + it.second < dist[it.first]){
                    dist[it.first] = wt + it.second;
                    pq.push({dist[it.first], it.first});
                }
            }
        }
        return dist;
    }
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<pair<int,int>>>adj(n+1);
        for(int i = 0; i<times.size();i++){
            auto u = times[i];
            adj[u[0]].push_back({u[1],u[2]});
        }

        vector<int>dist = dijkstras(k,adj,n);
        dist[0] = INT_MIN; // becuz numbered from 1 to n
        int maxi = INT_MIN;
        for(auto it:dist){
            maxi = max(maxi,it);
        }

        if(maxi==INT_MAX)
            return -1;
        else
            return maxi;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Dijsktra  | O(ElogV)        | O(V+E)           |
```

## Explanation

### 1. Intuition

```markdown
- Here we are given a graph with n nodes and times as directed edges.
- We need to find the minimum time it takes for all the n nodes to receive the signal.
- We can solve this problem using Dijkstra's algorithm.
- We can create an adjacency list to store the graph.
- We can create a vector to store the distance of each node from the source node.
- We can use a priority queue to store the nodes in increasing order of distance.
- We can start from the source node and update the distance of each node from the source node.
- Finally, we can return the maximum distance of any node from the source node.
- If the maximum distance is INT_MAX, then we can return -1.
- Otherwise, we can return the maximum distance.
```

### 2. Implementation

```markdown
- We can create a function `dijkstras` to implement Dijkstra's algorithm.
- We can initialize a vector `dist` to store the distance of each node from the source node.
- initialize `dist` with INT_MAX.
- Initialize `dist[src]` with 0.
- Create a priority queue `pq` to store the nodes in increasing order of distance.
- Push the source node into the priority queue.
- While the priority queue is not empty,
  we can pop the top node from the priority queue.
- For each adjacent node of the top node,
  we can update the distance of the adjacent node from the source node.
- If the updated distance is less than the current distance,
  we can update the distance and push the adjacent node into the priority queue.
- Finally, we can return the distance vector.
- In the main function, we can create an adjacency list to store the graph.
- We can iterate over the times array and add the edges to the adjacency list.
- We can call the `dijkstras` function with the source node and the adjacency list.
- We can find the maximum distance of any node from the source node.
- If the maximum distance is INT_MAX, then we can return -1.
- Otherwise, we can return the maximum distance.
```
