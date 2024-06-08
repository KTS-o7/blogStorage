+++
title = 'Problem Continuous Subarray Sum'
date = 2024-06-08T11:56:40+05:30
draft = false
series = 'leetcode'
tags =['hashmap','prefix-sum','prefixsum','vector']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 523](https://leetcode.com/problems/continuous-subarray-sum)

## Question

Given an integer array `nums` and an integer `k`, return `true` if `nums` has a **good subarray** or `false` otherwise.

A good subarray is a subarray where:

its length is at least two, and
the sum of the elements of the subarray is a multiple of **k**.

### Note

- A subarray is a contiguous part of the array.
- An integer x is a multiple of k if there exists an integer n such that x = n \* k. 0 is always a multiple of k.

## Example 1

```text
Input: nums = [23,2,4,6,7], k = 6
Output: true
Explanation: [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.
```

## Example 2

```text
Input: nums = [23,2,6,4,7], k = 6
Output: true
Explanation: [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.
```

## Example 3

```text
Input: nums = [23,2,6,4,7], k = 13
Output: false
```

## Constraints

```markdown
- `1 <= nums.length <= 10^5`
- `0 <= nums[i] <= 10^9`
- `0 <= sum(nums[i]) <= 23^1 - 1`
- `1 <= k <= 2^31 - 1`
```

## Solution

#### A Brute force Solution

```cpp
// This code is practically useless for larger values of k and nums;
class Solution {
    public:
    bool checkSubarraySum(vector<int>& nums,int k)
    {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        int size = nums.size();
        for(int i = 0; i<size; i++)
        {
            int sum = nums[i];
            for(int j = i+1; j<size; j++)
            {
                sum+=nums[j];
                if(k==0)
                {
                    if(sum==0)
                        return true;
                }
                else if(sum%k==0)
                    return true;
            }
        }
        return false;
    }
};
// Time complexity is O(N^2)
// Space complexity is O(1)
```

#### A Better Solution

```cpp
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
       std::ios::sync_with_stdio(false);
       cin.tie(nullptr);
       cout.tie(nullptr);

       int size = nums.size(), sum = 0;
       unordered_map<int,int>indexOccured;

       indexOccured[0] = -1;

       for(int i= 0; i<size; i++)
       {
            sum+=nums[i];

            if(indexOccured.find(sum%k) == indexOccured.end())
            {
                indexOccured[sum%k] = i;
            }
            else
            {
                int found = indexOccured[sum%k];
                if(i-found>1)
                    return true;
            }
       }
       return false;

    }
};
```

## Complexity Analysis

- `Time Complexity` : O(n)
- `Space Complexity` : O(n)

## Explanation

### 1. Intuition

```markdown
- We need to find a subarray whose sum is a multiple of k and length is atleast 2.
- We can use two loops to define start and end points of a subarray and calculate the sum.
- If the sum is a multiple of k, we can return true.
- But this solution is not optimal.
- We can use a hashmap to store the sum%k and the index where it occured.
  This works for fast lookups.
- So make it just a single loop to calculate the sum so far and store the sum%k in a hashmap.
- If the sum%k is already present in the hashmap, we can check if the difference between
  the current index and the index where the sum%k occured is greater than 1.
- if yes return true. If not continue.
```

### 2. Implementation

```markdown
- Intialize a hashmap `indexOccured` with key as 0 and value as -1.
- This initialization is to prevent the edge case where the first element is a multiple of k.
- Iterate over the array `nums` and calculate the `sum` so far.
- If the `sum%k` is not present in the hashmap, add it to the hashmap with the index.
- If the `sum%k` is already present in the hashmap:
  check if the difference between the current index and the index where the `sum%k` occured is greater than 1.
- If yes return true. If not continue.
- If we reach end of the loop, return false.
```

> A really good mathematical optimization problem. The hashmap is the key to this solution. The hashmap stores the sum%k and the index where it occured. If the sum%k is already present in the hashmap, we can check if the difference between the current index and the index where the sum%k occured is greater than 1.
