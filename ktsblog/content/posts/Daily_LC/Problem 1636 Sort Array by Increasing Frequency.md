+++
title = 'Problem 1636 Sort Array by Increasing Frequency'
date = 2024-07-23T21:12:33+05:30
draft = false
series = 'leetcode'
tags =['priority-queue','sorting','hashmap','custom-comparator']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1636](https://leetcode.com/problems/sort-array-by-increasing-frequency/description/)

## Question

Given an array of integers `nums`, sort the array in **increasing order** based on the frequency of the values. If multiple values have the same frequency, sort them in **decreasing order**.

Return the sorted array.

## Example 1

```
Input: nums = [1,1,2,2,2,3]
Output: [3,1,1,2,2,2]
Explanation: '3' has a frequency of 1, '1' has a frequency of 2,
 and '2' has a frequency of 3.
```

## Example 2

```
Input: nums = [2,3,1,3,2]
Output: [1,3,3,2,2]
Explanation: '2' and '3' both have a frequency of 2,
 so they are sorted in decreasing order.
```

## Example 3

```
Input: nums = [-1,1,-6,4,5,-6,1,4,1]
Output: [5,-1,4,4,-6,-6,1,1,1]
```

## Constraints

```markdown
- `1 <= nums.length <= 100`
- `-100 <= nums[i] <= 100`
```

## Solution

```cpp
class Solution {
public:
    vector<int> frequencySort(vector<int>& nums) {
        unordered_map<int,int>mp;
        for(int num:nums){
            mp[num]++;
        }
        auto cmp = [](const pair<int, int>& a, const pair<int, int>& b) {
            if (a.first == b.first) {
                return a.second < b.second;
            }
            return a.first > b.first;
        };

        priority_queue<pair<int,int>,vector<pair<int,int>>,decltype(cmp)>pq;

        for(auto it:mp){
            pq.push({it.second,it.first});
        }

        int ind = 0;
        pair<int,int>temp;
        while(!pq.empty()){
            temp =pq.top();
            pq.pop();
            while(temp.first>0){
                nums[ind++]=temp.second;
                temp.first--;
            }
        }
        return nums;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm                    | Time Complexity | Space Complexity |
| ---------------------------- | --------------- | ---------------- |
| Sorting using Priority Queue | O(NlogN)        | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- From the question we need to sort the array based on the frequency of the values.
- but if frequency is same then we need to sort them in decreasing order.
- Hence we need a mapping from value to frequency.
- We can use a hashmap to store the frequency of each value.
- Then we can use a priority queue to sort the values based on the frequency.
- Since we need a min heap we need to have a custom comparator.
```

### 2. Implementation

```markdown
- Initialize an unordered map `mp` to store the frequency of each value.
- Iterate over the array and store the frequency of each value.
- Initialize a custom comparator `cmp` to sort the values based on frequency.
- `cmp` will accept two value-frequency pairs
  - If the frequency of both values is same then return true if the value1 is less than value2.
  - Else return true if the frequency of value1 is greater than value2.
- Initialize a priority queue `pq` with the custom comparator `cmp`.
- Iterate over the map and push the value-frequency pair into the priority queue.
- Initialize an index `ind` to store the index of the sorted array.
- Initialize a temporary pair `temp` to store the value-frequency pair.
- Iterate over the priority queue until it is empty.
  - Pop the top element from the priority queue and store it in `temp`.
  - Iterate over the frequency of the value and store the value in the sorted array.
- Return the sorted array.
```
