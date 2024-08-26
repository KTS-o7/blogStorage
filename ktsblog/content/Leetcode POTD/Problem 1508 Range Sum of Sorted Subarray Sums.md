+++
title = 'Problem 1508 Range Sum of Sorted Subarray Sums'
date = 2024-08-04T17:16:54+05:30
draft = false
series = 'leetcode'
tags =['sorting','subarray','prefix-sum']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1508](https://leetcode.com/problems/range-sum-of-sorted-subarray-sums/description/)

## Question

You are given the array `nums` consisting of `n` positive integers. You computed the sum of all non-empty continuous subarrays from the array and then sorted them in non-decreasing order, creating a new array of `n * (n + 1) / 2` numbers.

Return the sum of the numbers from index `left` to index `right` (indexed from 1), inclusive, in the new array. Since the answer can be a huge number return it modulo `10^9 + 7`.

## Example 1

```
Input: nums = [1,2,3,4], n = 4, left = 1, right = 5
Output: 13
Explanation: All subarray sums are 1, 3, 6, 10, 2, 5, 9, 3, 7, 4. After sorting them in non-decreasing order we have the new array [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]. The sum of the numbers from index le = 1 to ri = 5 is 1 + 2 + 3 + 3 + 4 = 13.
```

## Example 2

```
Input: nums = [1,2,3,4], n = 4, left = 3, right = 4
Output: 6
Explanation: The given array is the same as example 1. We have the new array [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]. The sum of the numbers from index le = 3 to ri = 4 is 3 + 3 = 6.
```

## Example 3

```
Input: nums = [1,2,3,4], n = 4, left = 1, right = 10
Output: 50
```

## Constraints

```markdown
- `n == nums.length`
- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 100`
- `1 <= left <= right <= n * (n + 1) / 2`
```

## Solution

```cpp
class Solution {
public:
    int rangeSum(vector<int>& nums, int n, int left, int right) {
       vector<int>prefixSum(n);
       prefixSum[0]=nums[0];
       int mod = 1e9+7;
       for(int i = 1; i<n;i++){
        prefixSum[i] = prefixSum[i-1]+nums[i];
       }
        //for (auto it : prefixSum)
          //  cout<<it<<" ";
        //cout<<endl;
       vector<int>subSum;
       for(int i=0;i<n;i++){

        subSum.push_back(prefixSum[i]);
        for(int j =i+1;j<n;j++){
            subSum.push_back(prefixSum[j]-prefixSum[i]);
        }
       }

       sort(subSum.begin(),subSum.end());
       //for (auto it : subSum)
        //cout<<it<<" ";
       int sum = 0;
       for(int i = left-1;i<right;i++){
        sum = (sum+subSum[i])%mod;
       }
       return sum;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm                 | Time Complexity | Space Complexity |
| ------------------------- | --------------- | ---------------- |
| Subarray sum with sorting | O(n^2logn)      | O(n^2)           |
```

## Explanation

### 1. Intuition

```markdown
- We can generate the subarray sum using the prefix sum technique.
- Then sort the subarray sum and return the sum of the subarray sum from left to right.
- To generate subarray sum from index i to index j, we can use `prefixSum[j]-prefixSum[i]`.
```

### 2. Implementation

```markdown
- Initialize a vector `prefixSum` to store the prefix sum of the given array.
- Iterate over the array and store the prefix sum in the `prefixSum` array.
- Initialize a vector `subSum` to store the subarray sum.
- For each element in `prefixSum`, iterate over the array and calculate the subarray sum from index i to index j.
  - Subarray sum from i to j = `prefixSum[j]-prefixSum[i]`.
- Sort the `subSum` array.
- Initialize a variable `sum` to store the sum of the subarray sum from left to right.
- Iterate over the `subSum` array from left to right and calculate the sum.
  - Each addition should be modulo `1e9+7`.
  - This is to handle the overflow.
- return the sum.
```
