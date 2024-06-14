+++
title = 'Problem Minimum Increment to Make Array Unique'
date = 2024-06-14T21:23:14+05:30
draft = false
series = 'leetcode'
tags =['greedy','sorting','array']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 945](https://leetcode.com/problems/minimum-increment-to-make-array-unique/description/)

## Question

You are given an integer array `nums`. In one move, you can pick an index `i` where `0` <= `i` < `nums.length` and increment `nums[i]` by `1`.

Return the minimum number of moves to make every value in `nums` unique.

The test cases are generated so that the answer fits in a 32-bit integer.

## Example 1

```text
Input: nums = [1,2,2]
Output: 1
Explanation: After 1 move, the array could be [1, 2, 3].
```

## Example 2

```text
Input: nums = [3,2,1,2,1,7]
Output: 6
Explanation: After 6 moves, the array could be [3, 4, 1, 2, 5, 7].
```

## Constraints

- `0 <= nums[i] <= 10^5`
- `1 <= nums.length <= 10^5`

## Solution

```cpp
class Solution {
public:
    int minIncrementForUnique(vector<int>& nums) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        sort(nums.begin(),nums.end());
        int ans = 0,diff=0;
        for(int i = 1;i<nums.size();i++)
        {
            if(nums[i]<=nums[i-1])
            {
                diff = nums[i-1]-nums[i]+1;
                ans+= diff;
                nums[i]+=diff;
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(nlogn)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- If the array is sorted, we need to check the if current element is less than or equal to the previous element.
- If yes then we need to increment the current element by the difference of previous element and current element + 1.
- We add 1 just to make it unique.
```

### 2. Implementation

```markdown
- Sort the array.
- Initialize the answer `ans` and difference `diff` variable to 0.
- Iterate over the array from 1 to n.
- if `nums[i]` is less than or equal to `nums[i-1]` then calculate the difference.
- Difference is the difference between the previous element and the current element + 1.
- Increment the answer by the difference.
- Increment the current element by the difference.
- Return the answer.
```

### Alternate Approach

#### Code

```cpp
class Solution {
public:
    int minIncrementForUnique(vector<int>& nums) {
        int n = nums.size();
        int max_val = 0;
        int minIncrements = 0;

        // Find maximum value in nums using a loop
        for (int val : nums) {
            max_val = max(max_val, val);
        }

        // Create a frequencyCount vector to store the frequency of each value in nums
        vector<int> frequencyCount(n + max_val + 1, 0);

        // Populate frequencyCount vector with the frequency of each value in nums
        for (int val : nums) {
            frequencyCount[val]++;
        }

        // Iterate over the frequencyCount vector to make all values unique
        for (int i = 0; i < frequencyCount.size(); i++) {
            if (frequencyCount[i] <= 1) continue;

            // Determine excess occurrences, carry them over to the next value,
            // ensure single occurrence for current value, and update  minIncrements.
            int duplicates = frequencyCount[i] - 1;
            frequencyCount[i + 1] += duplicates;
            frequencyCount[i] = 1;
            minIncrements += duplicates;
        }
        return minIncrements;
    }
};

// Time Complexity: O(n + max_val)
// Space Complexity: O(n + max_val)
```

#### Approach : Counting

Another way to track duplicates is to use an array called frequencyCount. In this array, each index represents a unique value from our given array, nums, and the value at each index represents the count of occurrences of that value in nums.

For example: if 3 appears in nums twice, frequencyCount[3] would equal 2.

nums = [1,3,3,5,5]
frequencyCount = [0, 1, 0, 2, 0, 2]
We know nums contains all unique values when none of the values in frequencyCount is greater than 1.

Once we've created the frequencyCount array from nums, we can iterate through it and simulate the process used in Approach 1 to increment each duplicate value until all values become unique.

So elements with a count of 1 or less will remain unchanged. Upon encountering a duplicate, we'll calculate the surplus of elements with that value, carry that count to the next index, and set the current index value to 1.

We'll keep a running count for the number that we carry over to the next index; that equals how many moves it will take to make each value of nums unique.

We want to initialize frequencyCount with the largest possible range that could be needed to solve the problem. How do we determine this range?

The minimum length of frequencyCount would be the largest value in nums, and it must be long enough to hold the new values we get from incrementing any duplicates. Keep in mind that the maximum number of duplicates that we could possibly have is equal to the length of nums.

In problems like this, we can determine the longest possible length needed by considering a worst-case scenario. For instance, take the edge case where nums = [4, 4, 4, 4, 4].

The frequencyCount array for this would be:

frequencyCount = [0, 0, 0, 0, 5]

If we make every element unique, the frequencyCount array transforms to:

frequencyCount = [0, 0, 0, 0, 5, 6, 7, 8, 9]

As you can observe, the size of the frequencyCount array is 9, which equals the length of the original nums array plus the largest value found in nums.

#### Algorithm

- Initialize variables:
  - n as the length of nums.
  - max to store the maximum value in nums.
  - minIncrements to store the total number of increments needed.
- Find the maximum value in nums.
- Create an array frequencyCount to store the frequency of each element.
- Loop over nums and populate frequencyCount.
- Loop over the frequencyCount array. For each element:
  - If the frequency is less than or equal to one, continue with the next iteration.
  - Add the duplicates to the frequency of the next element.
  - Set the frequency of the current element to one.
  - Update minIncrements to account for the movement of the duplicates.
- Return minIncrements.

---
