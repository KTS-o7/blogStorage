+++
title = 'Problem 2191 Sort the Jumbled Number'
date = 2024-07-24T21:58:16+05:30
draft = false
series = 'leetcode'
tags =['priority-queue','hashmap','math','sorting','vectors']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2191](https://leetcode.com/problems/sort-the-jumbled-numbers/description/)

## Question

You are given a **0-indexed** integer array mapping which represents the mapping rule of a shuffled decimal system. `mapping[i] = j` means digit `i` should be mapped to digit `j` in this system.

The **mapped value** of an integer is the new integer obtained by replacing each occurrence of digit `i` in the integer with `mapping[i]` for all `0 <= i <= 9`.

You are also given another integer array `nums`. Return the array `nums` sorted in non-decreasing order based on the mapped values of its elements.

**Notes:**

- Elements with the same mapped values should appear in the same relative order as in the input.
- The elements of `nums` should only be sorted based on their mapped values and not be replaced by them.

## Example 1

```
Input: mapping = [8,9,4,0,2,1,3,5,7,6], nums = [991,338,38]
Output: [338,38,991]
Explanation:
Map the number 991 as follows:
1. mapping[9] = 6, so all occurrences of the digit 9 will become 6.
2. mapping[1] = 9, so all occurrences of the digit 1 will become 9.
Therefore, the mapped value of 991 is 669.
338 maps to 007, or 7 after removing the leading zeros.
38 maps to 07, which is also 7 after removing leading zeros.
Since 338 and 38 share the same mapped value, they should remain in the same relative order,
so 338 comes before 38.
Thus, the sorted array is [338,38,991].
```

## Example 2

```
Input: mapping = [0,1,2,3,4,5,6,7,8,9], nums = [789,456,123]
Output: [123,456,789]
Explanation: 789 maps to 789, 456 maps to 456, and 123 maps to 123.
Thus, the sorted array is [123,456,789].
```

## Constraints

```markdown
- `mapping.length == 10`
- `0 <= mapping[i] <= 9`
- `All the values of mapping[i] are unique.`
- `1 <= nums.length <= 3 * 10^4`
- `0 <= nums[i] < 10^9`
```

## Solution

```cpp
class Solution {
public:

    int convert(int temp,vector<int>& mapping){
            int change = 0;
            int ind = 1;
            while(temp>=0){
                change =  mapping[temp%10]*ind + change;
                ind*=10;
                temp/=10;
                if(temp == 0)
                    break;
            }
            return change;
    }

    vector<int> sortJumbled(vector<int>& mapping, vector<int>& nums) {
        std::ios::sync_with_stdio(false);
        unordered_map<int,int>rev;

        for(int i = 0; i<nums.size(); i++){
            rev[nums[i]] = i;
        }

        auto cmp = [&rev](const pair<int,int>&a, const pair<int,int>&b){
            if(a.first == b.first)
                return rev[a.second]>rev[b.second];
            return a.first>b.first;
        };

        priority_queue<pair<int,int>,vector<pair<int,int>>,decltype(cmp)>pq(cmp);

        int change;
        for(int i=0; i<nums.size(); i++){
            change = convert(nums[i],mapping);
            pq.push({change,nums[i]});
        }
        int i = 0;
        while(!pq.empty()){
            nums[i++] = pq.top().second;
            pq.pop();
        }
        return nums;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm      | Time Complexity | Space Complexity |
| -------------- | --------------- | ---------------- |
| Priority Queue | O(NlogN)        | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- The idea is as follows
  - Convert a number to its mapped value
  - Sort according to the mapped value
  - If the mapped values are the same, then choose order according to original index.
  - Return the sorted array of **Original Values**
- So we caan store the original index in a hashmap and use a priority queue to sort the mapped values.
- We can use a custom comparator to sort the priority queue.
- Finally, rebuild the array with the original values.
- Return the sorted array.
```

### 2. Implementation

```markdown
- Create a function `convert` to convert the number to its mapped value.

  - Input: a number `temp` to convert and a vector `mapping` to map the digits.
  - Output: the mapped value of the number.
  - `change` is the mapped value of the number.
  - `ind` is the multiplier to get the correct position of the digit.
  - While the number is greater than 0
    - Get the digit at the end of the number.
    - Multiply the digit with the mapped value and add it to the `change`.
    - Multiply the `ind` by 10 to get the correct position of the digit.
    - Divide the number by 10 to get the next digit.
    - If the number is 0, break the loop.
  - Return the `change`

- Create a hashmap `rev` to store the original index of the number.
- Iterate over the numbers and store the original index in the hashmap.
- Create a custom comparator `cmp` to sort the priority queue.
  - If the mapped values are the same, then choose the order according to the original index.
- Create a priority queue `pq` with the custom comparator `cmp`.
- Iterate over the numbers and convert the number to its mapped value.
- Push the mapped value and the original number to the priority queue.
- While the priority queue is not empty
  - Get the top element of the priority queue.
  - Pop the top element of the priority queue.
  - Store the original number in the sorted array.
- Return the sorted array.
```

### Alternate method

We can implement the same logic with vectors.

```cpp
class Solution {
public:

    int convert(int temp,vector<int>& mapping){
            if(temp == 0)
                return mapping[0];
            int change = 0;
            int ind = 1;
            while(temp>0){
                change =  mapping[temp%10]*ind + change;
                ind*=10;
                temp/=10;
            }
            return change;
    }

    vector<int> sortJumbled(vector<int>& mapping, vector<int>& nums) {
        int n = nums.size();
        vector<array<int,3>>triple(n); // using arrays for faster access;

        for(int i = 0; i<n; i++){
            int num = nums[i];
            triple[i] = {convert(num,mapping),i,num};
        }

        sort(triple.begin(),triple.end());
        for(int i = 0; i<n; i++){
            nums[i] = triple[i][2];
        }

        return nums;
    }
};
```

We can make it a little optimised by just storing the mapped value and index of original value in the vector.
then we have to create a new array of the original values and return it.

```cpp
class Solution {
public:

    int convert(int temp,vector<int>& mapping){
            if(temp == 0)
                return mapping[0];
            int change = 0;
            int ind = 1;
            while(temp>0){
                change =  mapping[temp%10]*ind + change;
                ind*=10;
                temp/=10;
            }
            return change;
    }

    vector<int> sortJumbled(vector<int>& mapping, vector<int>& nums) {
        int n = nums.size();
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        vector<array<int,2>>dbl(n); // using arrays for faster access;

        for(int i = 0; i<n; i++){
            int num = nums[i];
            dbl[i] = {convert(num,mapping),i};
        }

        sort(dbl.begin(),dbl.end());
        vector<int>ans(n);
        for(int i = 0; i<n; i++){
            ans[i] = nums[dbl[i][1]];
        }

        return ans;
    }
};
```
