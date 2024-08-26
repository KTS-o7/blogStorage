+++
title = 'Problem 1334 Find the City With the Smallest Number of Neighbours at a Threshold Distance'
date = 2024-07-26T15:00:02+05:30
draft = false
series = 'leetcode'
tags =['graphs','floyd-warshall','all-pair-shortest-path']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1334](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/description/)

## Question

There are `n` cities numbered from `0` to `n-1`. Given the array edges where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is at most `distanceThreshold`, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities i and j is equal to the sum of the edges' weights along that path.

## Example 1

{{<mermaid>}}
flowchart LR
A((0))-- 3 ---B((1))
B--1---C((2))
B--4---D((3))
C--1---D
{{</mermaid>}}

```
Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
Output: 3
Explanation: The figure above describes the graph.
The neighboring cities at a distanceThreshold = 4 for each city are:
City 0 -> [City 1, City 2]
City 1 -> [City 0, City 2, City 3]
City 2 -> [City 0, City 1, City 3]
City 3 -> [City 1, City 2]
Cities 0 and 3 have 2 neighboring cities at a distanceThreshold = 4,
but we have to return city 3 since it has the greatest number.
```

## Example 2

{{<mermaid>}}
flowchart LR
A((0))-- 2 ---B((1))
A--8---E((4))
B--3---C((2))
B--2---E
C--1---D((3))
D--1---E

{{</mermaid>}}

```
Input: n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
Output: 0
Explanation: The figure above describes the graph.
The neighboring cities at a distanceThreshold = 2 for each city are:
City 0 -> [City 1]
City 1 -> [City 0, City 4]
City 2 -> [City 3, City 4]
City 3 -> [City 2, City 4]
City 4 -> [City 1, City 2, City 3]
The city 0 has 1 neighboring city at a distanceThreshold = 2.
```

## Constraints

```markdown
- `2 <= n <= 100`
- `1 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 3`
- `0 <= fromi < toi < n`
- `1 <= weighti, distanceThreshold <= 10^4`
- All pairs `(fromi, toi)` are distinct.
```

## Solution

```cpp
class Solution {
public:
    vector<vector<int>> floydWarshal(int V,vector<vector<int>>&edges){
        int Lim = 1e9;
        vector<vector<int>> dist(V,vector<int>(V,Lim));
        for(int i = 0; i<V; i++){
            dist[i][i] = 0;
            for(const auto& edge:edges){
                dist[edge[0]][edge[1]] = edge[2];
                dist[edge[1]][edge[0]] = edge[2];
            }
        }

        for(int k = 0; k < V; k++){
            for(int i = 0; i < V; i++){
                for(int j = 0; j < V; j++){
                    if(dist[i][k] + dist[k][j] < dist[i][j]){
                        dist[i][j] = dist[i][k] + dist[k][j];
                    }
                }
            }
        }
        return dist;
    }

    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        std::ios::sync_with_stdio(false);
        vector<vector<int>> dist = floydWarshal(n,edges);
        int distCnt = 0,maxCity = INT_MIN,minCnt = INT_MAX;
        for(int i = 0; i<n;i++){
            distCnt = 0;
            for(int j =0;j<n;j++){
                if(dist[i][j]<=distanceThreshold){
                    distCnt+=1;
                }
            }
            if(distCnt<=minCnt){
                minCnt = distCnt;
                if(maxCity<i){
                    maxCity = i;
                }
            }
        }
        return maxCity;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm      | Time Complexity | Space Complexity |
| -------------- | --------------- | ---------------- |
| Flowd Warshall | O(V^3)          | O(V^2)           |
```

## Explanation

### 1. Intuition

```markdown
- We can use floyd warshall algorithm to find all pair shortest paths
- Then we can iterate through each node and count number of neighbours with distance less than
  or equal to the distance threshold.
- If the count is less than the count seen so far
  - update the count to new count
  - if the current city number is bigger than previous min, update city number
```

### 2. Implementation

```markdown
- Initialize the 'floydWarshal' function which takes the number of cities and edges as input
  - Initialize the distance matrix with a large number
  - Iterate through each edge and update the distance matrix
  - return the distance matrix
- Once the distance matrix is obtained, iterate through each city
  - Initialize the distance count to 0
  - Iterate through each city and check if the distance is less than or equal to the threshold
    - If yes, increment the distance count
  - If the distance count is less than or equal to the minimum count
    - Update the minimum count
    - If the current city number is greater than the previous minimum city number
      - Update the city number
- Return the city number
```

