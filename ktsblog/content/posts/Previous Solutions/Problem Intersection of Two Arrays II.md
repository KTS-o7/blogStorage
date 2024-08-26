+++
title = 'Problem 350 Intersection of Two Arrays II'
date = 2024-07-02T22:34:05+05:30
draft = false
series = 'leetcode'
tags =['arrays','hashmap','binary-search','sorting','two-pointer']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 350](https://leetcode.com/problems/intersection-of-two-arrays-ii/description/)

## Question

Given two integer arrays `nums1` and `nums2`, return an array of their intersection.
Each element in the result must appear as many times as it shows in both arrays and you may return the result in **any order**.

## Example 1

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

## Example 2

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
Explanation: [9,4] is also accepted.r
```

## Constraints

```
- 1 <= nums1.length, nums2.length <= 1000
- 0 <= nums1[i], nums2[i] <= 1000
```

## Solution

```cpp
// Iterative Solution
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    vector<int>freq(1001,0);
    for(int it:nums1){
        freq[it]++;
    }

    vector<int>ans;

    for(int it:nums2){
        if(freq[it]>0){
            ans.push_back(it);
            freq[it]--;
        }
    }
    return ans;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(n)
- `Space Complexity` : O(n)

## Explanation

### 1. Intuition

```markdown
- We just need to copy the repeated elements from both arrays to the resultant array.
- We can use a hashmap to store the frequency of elements in the first array.
- Then we can iterate over the second array and check if the element is present in the hashmap.
- If it is present, we can copy it to the resultant array and decrement the frequency in the hashmap.
- Finally, we return the resultant array.
```

### 2. Implementation

```markdown
- Initialize a hashmap `freq` to store the frequency of elements in the first array.
- Iterate over the first array and store the frequency of each element in the hashmap.
- Initialize an empty vector `ans` to store the resultant array.
- Iterate over the second array and check if the element is present in the hashmap.
- If it is present, copy it to the resultant array and decrement the frequency in the hashmap.
- Finally, return the resultant array.
```

#### Binary Search Solution

```cpp
// Binary Search Solution
class Solution {
public:
    static vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size()<nums2.size()) return intersect(nums2, nums1);
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        int sz=0, n1=nums1.size(), n2=nums2.size();
        for (int i = 0, j = 0; i < n1 && j < n2;) {
            int x=nums1[i], y=nums2[j];
            if (x==y){
                nums2[sz++] = x;
                i++;
                j++;
            }
            else if (x<y)// move i such that nums1[i]>=y
                i=lower_bound(nums1.begin()+i+1, nums1.end(), y)-nums1.begin();
            else  // x>y. Move j such that nums2[j]>=x
                j=lower_bound(nums2.begin()+j+1, nums2.end(), x)-nums2.begin();

        }
        nums2.resize(sz);
        return nums2;
    }
};
```

#### Explanation

```markdown
- We first sort both arrays.
- We then iterate over both arrays using two pointers.
- If the elements at the two pointers are equal, we copy the element to the resultant array and increment both pointers.
- If the element in the first array is less than the element in the second array,
  we move the first pointer to the next element that is greater than or equal to the element in the second array.
- If the element in the first array is greater than the element in the second array,
  we move the second pointer to the next element that is greater than or equal to the element in the first array.
- Finally, we resize the resultant array to the size of the intersection and return it.
```

```
Time Complexity: O(nlogn)
Space Complexity: O(1)
```
