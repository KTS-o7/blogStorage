+++
title = 'Problem 3275 K Th Nearest Obstacle Queries'
date = 2024-09-01T17:05:40+05:30
draft = false
series = 'leetcode'
tags =['priority-queue','math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3275](https://leetcode.com/problems/k-th-nearest-obstacle-queries/description/)

## Question

There is an infinite 2D plane.

You are given a positive integer `k`. You are also given a 2D array `queries`, which contains the following queries:

- `queries[i] = [x, y]`: Build an obstacle at coordinate `(x, y)` in the plane. It is guaranteed that there is no obstacle at this coordinate when this query is made.

After each query, you need to find the distance of the `k`<sup>th</sup> nearest obstacle from the origin.

Return an integer array `results` where `results[i]` denotes the `k`<sup>th</sup> nearest obstacle after query `i`, or `results[i] == -1` if there are less than `k` obstacles.

Note that initially there are no obstacles anywhere.

The distance of an obstacle at coordinate `(x, y)` from the origin is given by `|x| + |y|`.

## Example 1

```
Input: queries = [[1,2],[3,4],[2,3],[-3,0]], k = 2

Output: [-1,7,5,3]

Explanation:

Initially, there are 0 obstacles.
After queries[0], there are less than 2 obstacles.
After queries[1], there are obstacles at distances 3 and 7.
After queries[2], there are obstacles at distances 3, 5, and 7.
After queries[3], there are obstacles at distances 3, 3, 5, and 7.
```

## Example 2

```
Input: queries = [[5,5],[4,4],[3,3]], k = 1

Output: [10,8,6]

Explanation:

After queries[0], there is an obstacle at distance 10.
After queries[1], there are obstacles at distances 8 and 10.
After queries[2], there are obstacles at distances 6, 8, and 10.
```

## Constraints

```markdown
- `1 <= queries.length <= 2 * 10^5`
- `All queries[i] are unique.`
- `-10^9 <= queries[i][0], queries[i][1] <= 10^9`
- `1 <= k <= 10^5`
```

## Solution

```cpp
class Solution {
public:
    vector<int> resultsArray(vector<vector<int>>& queries, int k) {
        vector<int>res;
        priority_queue<int>pq;

        for (const auto& query : queries) {
            int x = query[0];
            int y = query[1];
            int dist = abs(x) + abs(y);

            if (pq.size() < k) {
                pq.push(dist);
            }
            else if (dist < pq.top()) {
                pq.pop();
                pq.push(dist);
            }

            if (pq.size() < k) {
                res.push_back(-1);
            }
            else {
                res.push_back(pq.top());
            }
        }

        return res;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm      | Time Complexity | Space Complexity |
| -------------- | --------------- | ---------------- |
| Priority Queue | O(N log K)      | O(K)             |
```

## Explanation

### 1. Intuition

- We need to keep track of obstacles being placed on a 2D plane and find the k<sup>th</sup> nearest obstacle to the origin after each query.
- The distance from the origin to any obstacle at coordinate (x, y) is given by the Manhattan distance: |x| + |y|.
- Initially, there are no obstacles. After each query, a new obstacle is added, and we need to update our result based on the current state of the plane.

1. Priority Queue (Max-Heap) Usage

   - To efficiently manage and retrieve the k<sup>th</sup> nearest obstacle, we use a max-heap (priority queue).
   - The heap will always store the distances of the nearest k obstacles to the origin.
   - The top of the max-heap will represent the k<sup>th</sup> nearest obstacle, as the heap property ensures that the largest of the k nearest distances is at the top.

2. Handling Each Query

   - For each query (x, y)
     - Calculate the Manhattan distance from the origin.
     - If the heap has fewer than k elements, simply add the distance to the heap.
     - If the heap already has k elements, compare the new distance with the top of the heap:
       - If the new distance is smaller, it should be part of the k closest distances, so replace the largest one (heap's top).
       - If not, discard the new distance as it’s not closer than the current k nearest distances.
     - After processing each query, check the heap's size:
     - If it has fewer than k elements, return -1 (indicating that there aren’t enough obstacles yet).
     - Otherwise, return the top of the heap, which is the k<sup>th</sup> nearest obstacle.

### 2. Implementation

```markdown
- Define a function `resultsArray` that takes the queries and k as input.
- Initialize an empty vector `res` to store the results.
- Create a max-heap (priority queue) `pq` to store the k nearest distances.
- Iterate over each query in the queries array:
  - Extract the x and y coordinates from the query.
  - Calculate the Manhattan distance from the origin.
  - If the heap size is less than k, add the distance to the heap.
  - If the heap size is k, compare the new distance with the top of the heap:
    - If the new distance is smaller, replace the top of the heap with the new distance.
  - If the heap size is less than k, add -1 to the results array.
  - Otherwise, add the top of the heap to the results array.
- Return the results array.
```
