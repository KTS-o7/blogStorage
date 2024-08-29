+++
title = 'Problem 947 Most Stones Removed With Same Row or Column'
date = 2024-08-29T21:35:03+05:30
draft = false
series = 'leetcode'
tags =['dfs','union-find','graph']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 947](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/description/)

## Question

On a 2D plane, we place `n` stones at some integer coordinate points. Each coordinate point may have at most one stone.

A stone can be removed if it shares either the same row or the same column as another stone that has not been removed.

Given an array `stones` of length `n` where `stones[i] = [xi, yi]` represents the location of the `i`<sup>th</sup> stone, return the largest possible number of stones that can be removed.

## Example 1

```
Input: stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
Output: 5
Explanation: One way to remove 5 stones is as follows:
1. Remove stone [2,2] because it shares the same row as [2,1].
2. Remove stone [2,1] because it shares the same column as [0,1].
3. Remove stone [1,2] because it shares the same row as [1,0].
4. Remove stone [1,0] because it shares the same column as [0,0].
5. Remove stone [0,1] because it shares the same row as [0,0].
Stone [0,0] cannot be removed since it does not share a row/column with another stone still on the plane.
```

## Example 2

```
Input: stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
Output: 3
Explanation: One way to make 3 moves is as follows:
1. Remove stone [2,2] because it shares the same row as [2,0].
2. Remove stone [2,0] because it shares the same column as [0,0].
3. Remove stone [0,2] because it shares the same row as [0,0].
Stones [0,0] and [1,1] cannot be removed since they do not share a row/column with another stone still on the plane.
```

## Example 3

```
Input: stones = [[0,0]]
Output: 0
Explanation: [0,0] is the only stone on the plane, so you cannot remove it.
```

## Constraints

- 1 <= `stones.length` <= 1000
- 0 <= `x`<sub>i</sub>, `y`<sub>i</sub> <= 10<sup>4</sup>
- No two stones are at the same coordinate point.

## Solution

```cpp
class Solution {
public:
    int removeStones(vector<vector<int>>& stones) {
        int n = stones.size();
        vector<vector<int>> adjlist(n);
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (stones[i][0] == stones[j][0] || stones[i][1] == stones[j][1]) {
                    adjlist[i].push_back(j);
                    adjlist[j].push_back(i);
                }
            }
        }

        int numOfGrps = 0;
        vector<bool> visited(n, false);

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(adjlist, visited, i);
                numOfGrps++;
            }
        }

        return n - numOfGrps;
    }

private:
    void dfs(vector<vector<int>>& adjlist,vector<bool>& visited, int stone) {
        visited[stone] = true;

        for (int neighbor : adjlist[stone]) {
            if (!visited[neighbor]) {
                dfs(adjlist, visited, neighbor);
            }
        }
    }
};
```

## Complexity Analysis

```markdown
| Algorithm               | Time Complexity | Space Complexity |
| ----------------------- | --------------- | ---------------- |
| Adjacency List creation | O(n^2)          | O(n^2)           |
| DFS                     | O(n)            | O(n)             |
| Overall                 | O(n^2)          | O(n^2)           |
```

## Explanation

### 1. Intuition

- Lets make a note of all the possible observations which might help us to build up a solution.
- We need to remove as many stones as possible.
- We can remove a stone only if it shares the same row or column with another stone.
- Basically the stone can be removed if its `connected` to another stone.
- We need to mathematically represent the `connected` stones. Such connections can be represented as a graph.
- Lets analyse how we can remove stones.

```
- If we have a stone at `[0,0]` and `[0,1]`, we can remove one of the stones.
- If we have a stone at `[0,0]` and `[1,0]`, we can remove one of the stones.
- If we have a stone at `[0,0]` and `[1,1]`, we cannot remove any stone.
```

- Generalizing the observation

```
- If we have a stone at `[x1,y1]` and `[x2,y2]`, we can remove one of the stones if `x1 == x2` or `y1 == y2`.
```

- One more observation is if there are `n` stones, we can remove `n-1` stones. This is because we can remove a stone only if its connected to another stone.
- By this logic, We cant remove if there is only one stone.
- Lets try to build a mathematical model to represent the stones and their connections.
- Mathematical approach to determine the number of stones that can be picked
- Trivial case is when there is only one stone, we cannot remove it.
- Lets assume there are `N` stones on the plane such that `N > 1`.
- If `N=2`, we can remove one stone.
- Inductive hypothesis is that we can remove `k` stones if there are `k+1` connected stones.
- then lets assume there is a stone `S` such that it is connected to `k` stones.
- We can remove `k` stones and `S` stone. So we can remove `k+1` stones.
- Hence it is proved that we can remove `k` stones if there are `k+1` connected stones.
- That means if there is `1` connected component with `k` stones, we can remove `k-1` stones.

In a plane there can be `n` stones which are distributed in `C` connected components. We can remove `n-C` stones.
The problem is reduced to finding the number of connected components in the graph.

```markdown
_TL DR_ :

- Build a graph of stones such that stones are connected if they share the same row or column.
- Find the number of connected components in the graph, using DFS.
- The number of stones that can be removed is `n - numOfConnectedComponents`.
```

### 2. Implementation

```markdown
- Intialize `n` as the number of stones.
- Create an adjacency list `adjlist` to represent the graph.
- For each stone `i` from `0` to `n-1`,
  - For each stone `j` from `i+1` to `n-1`,
    - If `stones[i][0] == stones[j][0]` or `stones[i][1] == stones[j][1]`,
      - Add `j` to the adjacency list of `i`.
      - Add `i` to the adjacency list of `j`.
- Intialize `numOfGrps` as `0`.
- Intialize a boolean vector `visited` of size `n` with all values as `false`.
- For each stone `i` from `0` to `n-1`,
  - If `visited[i]` is `false`,
    - Call `dfs` with `adjlist`, `visited` and `i`.
    - Increment `numOfGrps` by `1`.
- Return `n - numOfGrps`.

- Define a `dfs` function which takes `adjlist`, `visited` and `stone` as arguments.
  - Mark `stone` as `visited`.
  - For each `neighbor` in the adjacency list of `stone`,
    - If `neighbor` is not visited,
      - Call `dfs` with `adjlist`, `visited` and `neighbor`.
```

> There is a better union-find solution for this problem.

```cpp
class Disjoint {
public:
    vector<int> size, parent;
    Disjoint(int n) {
        for(int i = 0; i <= n; ++i) {
            size.push_back(1);
            parent.push_back(i);
        }
    }

    int findPar(int node) {
        if(node == parent[node]) return node;
        return parent[node] = findPar(parent[node]);
    }

    void unionn(int a, int b) {
        a = findPar(a);
        b = findPar(b);

        if(a == b) return;
        if(size[a] < size[b]) {
            parent[a] = b;
            size[b] += size[a];
        } else {
            parent[b] = a;
            size[a] += size[b];
        }
    }
};
class Solution {
public:
    int removeStones(vector<vector<int>>& stones) {
        int n = stones.size();

        int maxRow = 0, maxCol = 0;
        for(int i = 0; i < n; ++i) {
            maxRow = max(maxRow, stones[i][0]);
            maxCol = max(maxCol, stones[i][1]);
        }

        Disjoint *dsu = new Disjoint(maxRow + maxCol + 1);

        for(int i = 0; i < n; ++i) {
            int col = stones[i][1];
            int row = stones[i][0] + maxCol + 1;
            dsu -> unionn(row, col);
        }
        set<int> numComp;
        for(int i = 0; i < n; ++i) {
            int row = stones[i][0] + maxCol + 1;
            int col = stones[i][1];
            numComp.insert(dsu -> findPar(row));
            numComp.insert(dsu -> findPar(col));
        }

        return n - (int)numComp.size();

    }
};
```
