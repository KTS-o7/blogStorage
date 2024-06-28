+++
title = 'Problem Maximum Total Importance of Roads'
date = 2024-06-28T19:39:10+05:30
draft = false
series = 'leetcode'
tags =['sorting','undirected-graph','graph','vector']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2285](https://leetcode.com/problems/maximum-total-importance-of-roads/description/)

## Question

You are given an integer `n` denoting the number of cities in a country. The cities are numbered from `0` to `n - 1`.

You are also given a 2D integer array `roads` where `roads[i] = [ai, bi]` denotes that there exists a bidirectional road connecting cities `ai` and `bi`.

You need to assign each city with an integer value from `1` to `n`, where each value can only be used once. The importance of a road is then defined as the sum of the values of the two cities it connects.

**_Return the maximum total importance of all roads possible after assigning the values optimally._**

## Example 1

```
Input: n = 5, roads = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
Output: 43
Explanation: The figure above shows the country and the assigned values of [2,4,5,3,1].
- The road (0,1) has an importance of 2 + 4 = 6.
- The road (1,2) has an importance of 4 + 5 = 9.
- The road (2,3) has an importance of 5 + 3 = 8.
- The road (0,2) has an importance of 2 + 5 = 7.
- The road (1,3) has an importance of 4 + 3 = 7.
- The road (2,4) has an importance of 5 + 1 = 6.
The total importance of all roads is 6 + 9 + 8 + 7 + 7 + 6 = 43.
It can be shown that we cannot obtain a greater total importance than 43.
```

```
       1
     / | \
   0 --|-- 2 -- 4
    \  | /
       3
```

## Example 2

```
Input: n = 5, roads = [[0,3],[2,4],[1,3]]
Output: 20
Explanation: The figure above shows the country and the assigned values of [4,3,2,5,1].
- The road (0,3) has an importance of 4 + 5 = 9.
- The road (2,4) has an importance of 2 + 1 = 3.
- The road (1,3) has an importance of 3 + 5 = 8.
The total importance of all roads is 9 + 3 + 8 = 20.
It can be shown that we cannot obtain a greater total importance than 20.
```

```
      3          2
    /   \        |
   0     1       4
```

## Constraints

```markdown
- `2 <= n <= 5 * 10^4`
- `1 <= roads.length <= 5 * 10^4`
- `roads[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- There are no duplicate `roads`.
```

## Solution

```cpp
class Solution {
public:
    long long maximumImportance(int n, vector<vector<int>>& roads) {

        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        vector<long long int>freq(n,0);
        for(const vector<int>& it:roads)
        {
            freq[it[0]]++;
            freq[it[1]]++;
        }
        sort(freq.begin(),freq.end());
        long long int ans = 0;
        /*priority_queue<pair<long long int,int>>pq;

        for(int i = 0;i<n;i++)
           pq.push({freq[i],i});
        for(int i = n;i>0;i--)
        {
            ans += pq.top().first * i;
            pq.pop();
        }*/
        for(int i =n;i>0;i--)
        {
            ans+=freq[i-1]*i;
        }

        return ans;


    }
};
```

## Complexity Analysis

- `Time Complexity` : O(nlogn)
- `Space Complexity` : O(n)

## Explanation

### 1. Intuition

```markdown
- Looking at the problem we can see that importance of a node is directly proportional to the number of roads connected to it.
- So the node with highest number of roads must have higher importance.
- We know that each edge will connect 2 nodes and there are no duplicate edges.
- We can get the maximum sum when we assign the importance from `n` to `1` in the order of highly connected nodes to sparsely connected nodes.
```

### 2. Implementation

```markdown
- Initialize a frequency vector `freq` of size `n` with all elements as `0`.
- Iterate over the `roads` vector and increment the frequency of the nodes connected by the road.
- Sort the `freq` vector in ascending order.
- Initialize a variable `ans` of datatype `long long` to store the maximum importance.
- Iterate from the last element of the `freq` vector to the first element (`i=n to i=1`).
- Here `i` represents the importance of the node.
- `freq[i-1]` represents the number of roads connected to the `i-1`th node.
- Add the product of `freq[i-1]` and `i` to the `ans`
- Return the `ans`.
```

> This is a relatively simple problem where we need to assign importance to the nodes based on the number of roads connected to them. We can get the maximum importance by assigning the importance from `n` to `1` in the order of highly connected nodes to sparsely connected nodes.
