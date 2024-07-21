+++
title = 'Problem 1122 Relative Sort Array'
date = 2024-06-11T14:35:44+05:30
draft = false
series = 'leetcode'
tags =['vector','counting-sort','sorting','count-sort','hash-map']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1122](https://leetcode.com/problems/relative-sort-array/description/)

## Question

Given two arrays `arr1` and `arr2`, the elements of `arr2` are distinct, and all elements in `arr2` are also in `arr1`.

Sort the elements of `arr1` such that the relative ordering of items in `arr1` are the same as in `arr2`. Elements that do not appear in `arr2` should be placed at the end of `arr1` in ascending order.

## Example 1

```text
Input:
arr1 = [2,3,1,3,2,4,6,7,9,2,19],
arr2 = [2,1,4,3,9,6]
Output:
[2,2,2,1,4,3,3,9,6,7,19]
```

## Example 2

```text
Input:
arr1 = [28,6,22,8,44,17],
arr2 = [22,28,8,6]
Output: [22,28,8,6,17,44]
```

## Constraints

- `1 <= arr1.length, arr2.length <= 1000`
- `0 <= arr1[i], arr2[i] <= 1000`
- All the elements of `arr2` are **distinct**.
- Each `arr2[i]` is in `arr1`.

## Solution

```cpp
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        vector<int>freqCount(1001,0);
        for(int i = 0;i<arr1.size();i++)
        {
                freqCount[arr1[i]]++;
        }
        int ansptr=0;
        for(int i=0; i<arr2.size();i++)
        {
            while(freqCount[arr2[i]])
            {
                arr1[ansptr] = arr2[i];
                ansptr++;
                freqCount[arr2[i]]--;
            }
        }
        for(int i = 0;i<freqCount.size();i++)
        {
            while(freqCount[i])
            {
                arr1[ansptr] = i;
                ansptr++;
                freqCount[i]--;
            }
        }
        return arr1;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` : O(N)

## Explanation

### 1. Intuition

```markdown
- We need to know the count of elements in `arr1`
- Hence we use `freqCount` vector to store frequency counts.
- `arr2` will give us the order of insertion.
```

### 2. Implementation

```markdown
- First create a `freqCount` vector of size 1001 and initialize it with 0.
- Traverse through `arr1` and increment the frequency count of each element.
- Traverse through `arr2` and insert the elements in `arr1` in the order of `arr2`.
- Traverse through `freqCount` and insert the remaining elements in `arr1`.
- Now the array is sorted according to `arr2`.
- Return `arr1`.
```

### Note

#### Hashmap implementation

```cpp
class Solution{
    public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        map<int,int>freqCount;
        for(int i = 0;i<arr1.size();i++)
        {
            freqCount[arr1[i]]++;
        }
        int ansptr=0;
        for(int i=0; i<arr2.size();i++)
        {
            while(freqCount[arr2[i]])
            {
                arr1[ansptr] = arr2[i];
                ansptr++;
                freqCount[arr2[i]]--;
            }
        }
        for(auto i:freqCount)
        {
            while(i.second)
            {
                arr1[ansptr] = i.first;
                ansptr++;
                i.second--;
            }
        }
        return arr1;
    }
};
```
