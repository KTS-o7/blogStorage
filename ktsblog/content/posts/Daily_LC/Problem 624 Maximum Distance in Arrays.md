+++
title = 'Problem 624 Maximum Distance in Arrays'
date = 2024-08-16T19:52:59+05:30
draft = false
series = 'leetcode'
tags =['array','greedy']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 624](https://leetcode.com/problems/maximum-distance-in-arrays/description/)

## Question

You are given `m` `arrays`, where each array is sorted in ascending order.

You can pick up two integers from two different arrays (each array picks one) and calculate the distance. We define the distance between two integers `a` and `b` to be their absolute difference `|a - b|`.

Return the maximum distance.

#### Note

Even though the dont mention in the problem statement, the integers `a` and `b` CANT be from the same array.
Also the arrays can have different lengths and are not arranged in any particular order.
But every array is sorted in ascending order in itself.

## Example 1

```
Input: arrays = [[1,2,3],[4,5],[1,2,3]]
Output: 4
Explanation: One way to reach the maximum distance 4 is to pick 1 in the first or third array and pick 5 in the second array.
```

## Example 2

```
Input: arrays = [[1],[1]]
Output: 0
```

## Example 3

```
Edge case
Input: arrays = [[1,4],[0,5]]
Output: 4
```

## Constraints

```markdown
- `m == arrays.length`
- `2 <= m <= 10^5`
- `1 <= arrays[i].length <= 500`
- -10^4 <= arrays[i][j] <= 10^4`
- `arrays[i]` is sorted in ascending order.
- There will be at most `10^5` integers in all the arrays.
```

## Solution

```cpp
class Solution {
public:
    int maxDistance(vector<vector<int>>& arrays) {
        int small = arrays[0].front();
        int big = arrays[0].back();
        int maxDist = 0;

        for (int i = 1; i < arrays.size(); ++i) {
            maxDist = max(maxDist, abs(arrays[i].back() - small));
            maxDist = max(maxDist, abs(big - arrays[i].front()));
            small = min(small, arrays[i].front());
            big = max(big, arrays[i].back());
        }

        return maxDist;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm          | Time Complexity | Space Complexity |
| ------------------ | --------------- | ---------------- |
| Greedy single pass | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- Since the arrays are sorted, we need to only check the first and last element of each array.
- Since we need to pick elements from different arrays, we need to keep track of the smallest and biggest element we have seen so far.
- Always calculate the distance between the current array's first element and the biggest element we have seen so far
  and the distance between the current array's last element and the smallest element we have seen so far.
- Store the maximum distance seen so far.
- Update the smallest and biggest element we have seen so far.
- even though the arrays are not sorted in any particular order, we can still find the maximum distance by keeping track of the smallest and biggest element we have seen so far.
```

### 2. Implementation

```markdown
- Initialize integers `small` and `big` to store the smallest and biggest element of the first array.
- Initialize an integer `maxDist` to store the maximum distance seen so far.
- Iterate over the arrays from 2nd array to the last array.
- Calculate the distance between the current array's last element and `small` and update `maxDist`.
- Calculate the distance between the current array's first element and `big` and update `maxDist`.
- Update `small` to store the minimum of `small` and the current array's first element.
- Update `big` to store the maximum of `big` and the current array's last element.
- Return `maxDist`.
```

> On simple glance it would look like we are picking the smallest and biggest element from the same array, but we are not. We are picking the smallest and biggest element from different arrays.
> this is because the smallest and biggest element we have seen so far are updated only when we find a new smallest or biggest element in the next array.
> The mathematical proof is as follows

### Proof of Correctness for Greedy Approach

#### Assumptions

1. Each array is sorted in non-decreasing order.
2. We are given multiple arrays A1, A2, ..., An.
3. We need to find the maximum distance between any two elements such that each element comes from a different array.

#### Proof

**Base Case**:
For the first array (A1), `S` is initialized to its first element, and `L` to its last element. This ensures that the initial distance calculation considers the full range of possible values within the first array.

**Inductive Step**:
Assume that after processing the first `k` arrays, `S` and `L` hold the smallest and largest elements among those processed, and `D_max` holds the maximum distance found so far.

When processing the (k+1)-th array (Ak+1):

1. Calculate the potential distances:

   - `D1 = abs(Ak+1[0] - L)`
   - `D2 = abs(S - Ak+1[n-1])`

2. Immediately update `S` and `L` if necessary:

   - If `Ak+1[0] < S`, then `S = Ak+1[0]`.
   - If `Ak+1[n-1] > L`, then `L = Ak+1[n-1]`.

3. At this point, `D1` and `D2` are already calculated based on the old values of `S` and `L`. We do not recalculate them after updating `S` and `L`; instead, we directly compare them against `D_max` to decide whether to update `D_max`.

**Conclusion**:
By the end of the iteration, `S` and `L` will represent the smallest and largest elements from different arrays. The global maximum distance will be the maximum of all `D1` and `D2` values calculated during the iteration.

Thus, the greedy approach ensures that we always consider the most extreme values from different arrays, guaranteeing the maximum possible distance.
