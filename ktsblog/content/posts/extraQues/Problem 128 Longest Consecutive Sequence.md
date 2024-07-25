+++
title = 'Problem 128 Longest Consecutive Sequence'
date = 2024-07-25T21:47:34+05:30
draft = false
series = 'leetcode'
tags =['hashmap','count','set','hashset']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 128](https://leetcode.com/problems/longest-consecutive-sequence/description/)

## Question

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in `O(n)` time.

## Example 1

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4].
Therefore its length is 4.

```

## Example 2

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

## Constraints

```markdown
- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
```

## Solution

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        std::ios::sync_with_stdio(false);
        unordered_set<int>s(nums.begin(),nums.end());
        int ans = 0,len =1;
        for(int num:nums){
            if(s.find(num-1)==s.end()){
                len = 1;
                while(s.find(num+len)!=s.end()){
                    len++;
                }
                ans = max(ans,len);
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Hash set  | O(N)            | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- We can add all the elements of the array to a hash set.
- Then we can say that if the previous element of the current element is not present in the hash set, then we can start counting the length of the sequence.
- Only elements whose previous element is not present in the hash set can be the starting element of the sequence.
- Using this approach, we can find how many consecutive elements are present in the sequence which starts from the current element.
- We can keep track of the maximum length of the sequence and return it.
```

### 2. Implementation

```markdown
- Initialize `s` a hash set to store all the elements of the array.
- Initialize `ans` to 0 and `len` to 1 to store the maximum length of the sequence and the length of the current sequence.
- Iterate through the array and do the following
  - If the previous element of the current element is not present in the hash set, then
    - Set `len` to 1
    - While the next element of the current element is present in the hash set, increment `len`
    - Set `ans` to the maximum of `ans` and `len`
- Return `ans`
```

> This solution was found by a random youtube short by neetcode. So huge shoutout to him !
