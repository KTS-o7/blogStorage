+++
title = 'Problem 2976 Minimum Cost to Convert String I'
date = 2024-07-27T15:35:56+05:30
draft = false
series = 'leetcode'
tags =['floyd-warshal','graph','shortest-path','all-pair-shortest-path']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem ](https://leetcode.com/problems/minimum-cost-to-convert-string-i/description/)

## Question

You are given two 0-indexed strings `source` and `target`, both of length `n` and consisting of lowercase English letters. You are also given two 0-indexed character arrays `original` and `changed`, and an integer array `cost`, where `cost[i]` represents the cost of changing the character `original[i]` to the character `changed[i]`.

You start with the string `source`. In one operation, you can pick a character `x` from the string and change it to the character `y` at a cost of `z` if there exists any index `j` such that `cost[j] == z`, `original[j] == x`, and `changed[j] == y`.

Return the **minimum cost** to convert the string source to the string target using any number of operations. If it is impossible to convert source to target, return `-1`.

Note that there may exist indices `i`, `j` such that `original[j] == original[i]` and `changed[j] == changed[i]`.

#### Note

One of the edge case is when the there are different weights for the same character in the original and changed array. In that case, we need to consider the minimum cost.

This is crucial when using the Floyd-Warshall algorithm.

## Example 1

```
Input: source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"],
changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]
Output: 28
Explanation: To convert the string "abcd" to string "acbe":
- Change value at index 1 from 'b' to 'c' at a cost of 5.
- Change value at index 2 from 'c' to 'e' at a cost of 1.
- Change value at index 2 from 'e' to 'b' at a cost of 2.
- Change value at index 3 from 'd' to 'e' at a cost of 20.
The total cost incurred is 5 + 1 + 2 + 20 = 28.
It can be shown that this is the minimum possible cost.
```

## Example 2

```
Input: source = "aaaa", target = "bbbb", original = ["a","c"],
changed = ["c","b"], cost = [1,2]
Output: 12
Explanation: To change the character 'a' to 'b' change the character 'a' to 'c' at a cost of 1,
followed by changing the character 'c' to 'b' at a cost of 2,
for a total cost of 1 + 2 = 3. To change all occurrences of 'a' to 'b',
a total cost of 3 * 4 = 12 is incurred.
```

## Example 3

```
Input: source = "abcd", target = "abce", original = ["a"],
changed = ["e"], cost = [10000]
Output: -1
Explanation: It is impossible to convert source to target
because the value at index 3 cannot be changed from 'd' to 'e'.
```

## Constraints

```markdown
- `1 <= source.length == target.length <= 10^5`
- `source`, `target` consist of lowercase English letters.
- `1 <= cost.length == original.length == changed.length <= 2000`
- `original[i]`, `changed[i]` are lowercase English letters.
- `1 <= cost[i] <= 10^6`
- `original[i] != changed[i]`
```

## Solution

```cpp
class Solution {
public:
vector<vector<long long>> floydWarshal(vector<char>& original,vector<char>& changed, vector<int>& cost){
    vector<vector<long long>>minCost(26,vector<long long>(26,INT_MAX));
    int s,t;
    for(int i = 0; i<original.size(); i++){
        s = original[i]-'a';
        t = changed[i]-'a';
        minCost[s][t]=min(minCost[s][t],(long long)cost[i]); // This is needed because there may be repeated edges with different weights
        }


    for(int k =0;k<26;k++){
        for(int i=0;i<26;i++){
            for(int j= 0; j<26;j++){
                if(minCost[i][j]>minCost[i][k]+minCost[k][j]){
                    minCost[i][j] = minCost[i][k]+minCost[k][j];
                }
            }
        }
    }
    return minCost;
}

    long long minimumCost(string source, string target, vector<char>& original, vector<char>& changed, vector<int>& cost) {
        vector<vector<long long>>minCost = floydWarshal(original,changed,cost);
        long long totCost = 0;
        int s,t;
        for(int i =0;i<source.size();i++){
            if(source[i]!=target[i]){
                s = source[i]-'a';
                t = target[i]-'a';
                if(minCost[s][t]==INT_MAX)
                    return -1;
                totCost+=minCost[s][t];
            }
        }
        return totCost;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm     | Time Complexity | Space Complexity |
| ------------- | --------------- | ---------------- |
| Floyd-Warshal | O(26^3)         | O(26^2)          |
```

## Explanation

### 1. Intuition

```markdown
- Since we need to check if we can derive one character from another, we can model this problem as a graph where the nodes are the characters and the edges are the cost of changing one character to another.
- The cost to convert character `a` to character `b` is the minimum path from `a` to `b` in the graph.
- It need not to be the immediate connection between `a` and `b`.
- Since we have 26 characters, we have 26x26 matrix to store the cost of changing one character to another.
- Hence we can use the Floyd-Warshall algorithm to find the minimum cost to convert the string.
- First build cost matrix using the original and changed array.
- Then use the Floyd-Warshall algorithm to find the minimum cost to convert the string.
- Then for each character in source and target
  1. if they are not equal, find the cost to convert the character from source to target.
  2. If the cost is INT_MAX, return -1.
  3. Else add the cost to the total cost.
- Return the total cost.
```

### 2. Implementation

```markdown
- Define the `floydWarshal` function which takes the original, changed and cost array as input
  - Initialize the cost matrix with INT_MAX
  - for each edge `minCost[s][t] = min(minCost[s][t],cost[i])`
  - This is because there may be repeated edges with different weights
  - Use the Floyd-Warshall algorithm to find the minimum cost to convert the string
  - Return the cost matrix
- Once the cost matrix is obtained, iterate through each character in source and target
  - If the characters are not equal
    - Find the cost to convert the character from source to target
    - If the cost is INT_MAX, return -1
    - Else add the cost to the total cost
- Return the total cost
```

> The intuition to convert this word problem to a graph problem is crucial. This is a common pattern in graph problems where we need to find the minimum cost to convert one thing to another.
