+++
title = 'Problem 719 Find K Th Smallest Pair Distance'
date = 2024-08-14T18:02:48+05:30
draft = false
series = 'leetcode'
tags =['sorting','priority-queue','binary-search']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 719](https://leetcode.com/problems/find-k-th-smallest-pair-distance/)

## Question

The distance of a pair of integers `a` and `b` is defined as the absolute difference between `a` and `b`.

Given an integer array `nums` and an integer `k`, return the `kth` smallest distance among all the pairs `nums[i]` and `nums[j]` where `0 <= i < j < nums.length`.

## Example 1

```
Input: nums = [1,3,1], k = 1
Output: 0
Explanation: Here are all the pairs:
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
Then the 1st smallest distance pair is (1,1), and its distance is 0.
```

## Example 2

```
Input: nums = [1,1,1], k = 2
Output: 0
```

## Example 3

```
Input: nums = [1,6,1], k = 3
Output: 5
```

## Constraints

```markdown
- `n == nums.length`
- `2 <= n <= 10^4`
- `0 <= nums[i] <= 10^6`
- `1 <= k <= n * (n - 1) / 2`
```

## Solution

```cpp
// Brute Force
class Solution {
public:
    int smallestDistancePair(vector<int>& nums, int k) {
        std::ios::sync_with_stdio(false);
        priority_queue<int,vector<int>,greater<int>>pq;
        for(int i=0;i<nums.size();i++){
            for(int j= i+1; j<nums.size();j++){
                pq.push(abs(nums[i]-nums[j]));
            }
        }

        for(int i = 1; i<k; i++)
            pq.pop();
        return pq.top();
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Pair      | O(N^2)          | O(1)             |
| Priority  | O(NlogN)        | O(N)             |
```

## Better Solution

```cpp
// Binary Search
class Solution {
public:
    static int cntPairs(int x, vector<int>& nums){
        const int n=nums.size();
        int cnt=0;
        for(int l=0, r=1; r<n; r++){
            while(r>l && nums[r]-nums[l]>x)
                l++;
            cnt+=r-l;
        }
        return cnt;
    }

    static int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int l=0, r=nums.back()-nums.front(), m;
        while(l<r){
            m=(r+l)/2;
            if (cntPairs(m, nums)<k)
                l=m+1;
            else r=m;
        }
        return l;
    }
};

auto init = []() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    return 'c';
}();
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Binary    | O(NlogN)        | O(1)             |
| Count     | O(N)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- First approach is quite straight forward
- Lets discuss the Binary Search approach
- Since we need to find the kth smallest distance between the pairs
  We need to estimate how many pairs can have the distance less than or equal to `k`.
- We know that the maximum distance between the pairs is
  the difference between the maximum and minimum element in the array.
- So the boundaries of the binary search are `0` and the `maximum difference`.
- We calculate the mid of the boundaries and check how many pairs have the distance less than or equal to the mid.
- If the number of pairs is less than `k`, we move the left boundary to mid+1.
- If the number of pairs is greater than or equal to `k`, we move the right boundary to mid.
- We return the left boundary as the answer.
```

### 2. Implementation

```markdown
- Intialize `countPairs` function to calculate the number of pairs with distance less than or equal to `x`.
  - Given the sorted array and the distance `x`,
    initialize the count to `0`.
  - Iterate over the array with two pointers `l` from 0 and `r` from 1.
  - If the difference between the elements at `r` and `l` is greater than `x`,
    increment the left pointer `l`.
  - Add the difference between `r` and `l` to the count.
  - Increment the right pointer `r`.
    - Return the count.
- Initialize the `smallestDistancePair` function to find the kth smallest distance.
  - Sort the array to get the maximum and minimum elements.
  - Initialize the left boundary `l` to `0` and the right boundary `r` to the difference between the maximum and minimum elements.
  - Iterate until the left boundary is less than the right boundary.
    - Calculate the mid of the boundaries.
    - If the number of pairs with distance less than or equal to the mid is less than `k`,
      move the left boundary to mid+1.
    - Otherwise, move the right boundary to mid.
  - Return the left boundary as the answer.
```
