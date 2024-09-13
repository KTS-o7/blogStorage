+++
title = 'Problem 1310 XOR Queries of a Subarray'
date = 2024-09-13T10:33:51+05:30
draft = false
series = 'leetcode'
tags =['xor','prefix-sum','bit-manipulation','array','range-query']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1310](https://leetcode.com/problems/xor-queries-of-a-subarray/description/)

## Question

You are given an array `arr` of positive integers. You are also given the array `queries` where `queries[i] = [lefti, righti]`.

For each query `i` compute the XOR of elements from `lefti` to `righti` (that is, ` arr[lefti] XOR arr[lefti + 1] XOR ... XOR arr[righti]` ).

Return an array `answer` where `answer[i]` is the answer to the `ith` query.

## Example 1

```
Input: arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]
Output: [2,7,14,8]
Explanation:
The binary representation of the elements in the array are:
1 = 0001
3 = 0011
4 = 0100
8 = 1000
The XOR values for queries are:
[0,1] = 1 xor 3 = 2
[1,2] = 3 xor 4 = 7
[0,3] = 1 xor 3 xor 4 xor 8 = 14
[3,3] = 8
```

## Example 2

```
Input: arr = [4,8,2,10], queries = [[2,3],[1,3],[0,0],[0,3]]
Output: [8,0,4,4]
```

## Constraints

- 1 <= `arr.length` <= 3 \* 10<sup>4</sup>
- 1 <= `arr[i]` <= 10<sup>9</sup>
- `queries[i].length` == 2
- 0 <= `queries[i][0]` <= `queries[i][1] < arr.length`

## Solution

```cpp
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
        int n = arr.size();
        vector<int> prefixXOR(n);
        vector<int> ans;

        prefixXOR[0] = arr[0];
        for (int i = 1; i < n; i++) {
            prefixXOR[i] = prefixXOR[i - 1] ^ arr[i];
        }

        for (int i = 0; i < queries.size(); i++) {
            if (queries[i][0] == 0) {
                ans.push_back(prefixXOR[queries[i][1]]);
            }
            else {
                ans.push_back(prefixXOR[queries[i][1]] ^ prefixXOR[queries[i][0] - 1]);
            }
        }

        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm     | Time Complexity | Space Complexity |
| ------------- | --------------- | ---------------- |
| Prefix XOR    | O(n)            | O(n)             |
| Range compute | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

The question wants us to find the XOR of elements from `lefti` to `righti` for each query.

By the property of XOR, we know that `a XOR b XOR a = b`.
Hence XOR of elements from `lefti` to `righti` is equal to `0 to righti XOR 0 to lefti-1`.

By this property, we can compute any query by using the prefix XOR array.

### 2. Implementation

```markdown
- Initialize a variable `n` to store the size of the array.
- Initialize a vector `prefixXOR` of size `n` to store the prefix XOR of the array.
- Initialize a vector `ans` to store the answer of the queries.
- Compute the prefix XOR of the array in the following manner
  - Initialize the `prefixXOR[0]` as `arr[0]`.
  - for `i` from `1` to `n-1`, compute `prefixXOR[i] = prefixXOR[i-1] XOR arr[i]`.
- For each query in the `queries` array, compute the answer in the following manner
  - If `queries[i][0]` is `0`, then the answer is `prefixXOR[queries[i][1]]`.
  - Else the answer is `prefixXOR[queries[i][1]] XOR prefixXOR[queries[i][0]-1]`.
- Return the answer vector.
```
