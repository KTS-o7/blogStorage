+++
title = 'Problem 3289 the Two Sneaky Numbers of Digitville'
date = 2024-09-15T19:27:50+05:30
draft = false
series = 'leetcode'
tags =['matrix','math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3289](https://leetcode.com/problems/the-two-sneaky-numbers-of-digitville/description/)

## Question

In the town of Digitville, there was a list of numbers called `nums` containing integers from `0` to `n - 1`. Each number was supposed to appear exactly once in the list, however, two mischievous numbers sneaked in an additional time, making the list longer than usual.

As the town detective, your task is to find these two sneaky numbers. Return an array of size two containing the two numbers (in any order), so peace can return to Digitville.

## Example 1

```
Input: nums = [0,1,1,0]

Output: [0,1]

Explanation:

The numbers 0 and 1 each appear twice in the array.
```

## Example 2

```
Input: nums = [0,3,2,1,3,2]

Output: [2,3]

Explanation:

The numbers 2 and 3 each appear twice in the array.
```

## Example 3

```
Input: nums = [7,1,5,4,3,4,6,0,9,5,8,2]

Output: [4,5]

Explanation:

The numbers 4 and 5 each appear twice in the array.
```

## Constraints

- `2 <= n <= 100`
- `nums.length == n + 2`
- `0 <= nums[i] < n`
- The input is generated such that `nums` contains exactly two repeated elements.

## Solution

```cpp
class Solution {
public:
   vector<int> getSneakyNumbers(vector<int>& nums) {
       int n = nums.size() - 2;
       vector<int> count(n, 0);
       vector<int> result;

        for (int num : nums) {
            count[num]++;
        }

        for (int i = 0; i < n; i++) {
            if (count[i] == 2) {
                result.push_back(i);
            }
        }

        return result;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| counting  | O(n)            | O(n)             |
| traversal | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

The problem is fairly simple.

- According to the question a number is sneaky if its repeated twice.
- So count the frequency of each number and check if the frequency is 2.
- If the frequency is 2 then push the number in the result array.
- Return the result array.

### 2. Implementation

```markdown
- Initialize a vector count of size `n-2` with 0.
- Initialize a vector `result` to store the sneaky numbers.
- Traverse the `nums` array and increment the count of each number.
- Traverse the count array and check if the frequency is 2.
- If the frequency is 2 then push the number in the `result` array.
- Return the `result` array.
```
