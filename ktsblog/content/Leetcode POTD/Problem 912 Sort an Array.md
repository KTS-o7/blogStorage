+++
title = 'Problem 912 Sort an Array'
date = 2024-07-25T12:20:26+05:30
draft = false
series = 'leetcode'
tags =['sorting']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 912](https://leetcode.com/problems/sort-an-array/description/)

## Question

TLDR: Given an integer array `nums`, sort it in ascending order. using merge sort.

## Example 1

```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
Explanation: After sorting the array, the positions of some numbers are not changed (for example, 2 and 3),
while the positions of other numbers are changed (for example, 1 and 5).
```

## Example 2

```
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
Explanation: Note that the values of nums are not necessairly unique.
```

## Constraints

```markdown
- `1 <= nums.length <= 5 * 10^4`
- `-5 * 10^4 <= nums[i] <= 5 * 10^4`
```

## Solution

```cpp
class Solution {
public:
    void merge(vector<int>& arr, int left, int mid, int right){
    int n1 = mid - left + 1;
    int n2 = right - mid;
    vector<int> L(n1), R(n2);
    for(int i = 0; i < n1; i++){
        L[i] = arr[left + i];
    }
    for(int i = 0; i < n2; i++){
        R[i] = arr[mid + 1 + i];
    }
    int i = 0, j = 0, k = left;
    while(i < n1 && j < n2){
        if(L[i] <= R[j]){
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    while(i < n1){
        arr[k] = L[i];
        i++;
        k++;
    }
    while(j < n2){
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(vector<int>& arr, int left, int right){
    if(left < right){
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}
    vector<int> sortArray(vector<int>& nums) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        mergeSort(nums,0,nums.size()-1);
        return nums;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Mergesort | O(NlogN)        | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- Break the array into two halves
- Sort the two halves
- Merge the two halves
```

### 2. Implementation

```markdown
- Create a merge function that merges two sorted arrays
- Create a mergeSort function that breaks the array into two halves and sorts them
- Call the mergeSort function
- Return the sorted array
```

> Nothing fancy here, just a simple merge sort implementation.
