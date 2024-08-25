+++
title = 'Problem 3265 Count Almost Equal Pairs I'
date = 2024-08-25T12:33:25+05:30
draft = false
series = 'leetcode'
tags =['string','count']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3265](https://leetcode.com/problems/count-almost-equal-pairs-i/description/)

## Question

You are given an array `nums` consisting of positive integers.

We call two integers `x` and `y` in this problem almost equal if both integers can become equal after performing the following operation at most once:

- Choose either `x` or `y` and swap any two digits within the chosen number.

Return the number of indices `i` and `j` in nums where `i < j` such that `nums[i]` and `nums[j]` are almost equal.

Note that it is allowed for an integer to have leading zeros after performing an operation.

## Example 1

```
Input: nums = [3,12,30,17,21]

Output: 2

Explanation:

The almost equal pairs of elements are:

3 and 30. By swapping 3 and 0 in 30, you get 3.
12 and 21. By swapping 1 and 2 in 12, you get 21.
```

## Example 2

```
Input: nums = [1,1,1,1,1]

Output: 10

Explanation:

Every two elements in the array are almost equal.
```

## Example 3

```
Input: nums = [123,231]

Output: 0

Explanation:

We cannot swap any two digits of 123 or 231 to reach the other.
```

## Constraints

```markdown
- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 10^6`
```

## Solution

```cpp
class Solution {
public:
    bool helper(int a, int b) {
        string sa = to_string(a);
        string sb = to_string(b);

        int maxLen = max(sa.length(), sb.length());
        sa = string(maxLen - sa.length(), '0') + sa;
        sb = string(maxLen - sb.length(), '0') + sb;

        if (sa == sb) return true;

        int diffCount = 0;
        int firstDiff = -1, secondDiff = -1;

        for (int i = 0; i < maxLen; i++) {
            if (sa[i] != sb[i]) {
                diffCount++;
                if
                    (diffCount == 1) firstDiff = i;
                else if
                    (diffCount == 2) secondDiff = i;
                else
                    return false;
            }
        }

        return diffCount == 2 && sa[firstDiff] == sb[secondDiff] && sa[secondDiff] == sb[firstDiff];
    }

    int countPairs(vector<int>& nums) {
        int count = 0;
        int n = nums.size();

        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (helper(nums[i], nums[j])) {
                    count++;
                }
            }
        }
        return count;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Main loop | O(n^2)          | O(1)             |
| Helper    | O(maxLen)       | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- We need to take two numbers (along with preceeding zeros) and check if they are almost equal.
- A number can be almost equal to another number
  if we can swap any two digits of the number to make it equal to the other number.
- If more than two digits are different, then we cannot make them equal.
- And we can swap only once.
- So, we need to check if we can swap any two digits of the number
  to make it equal to the other number.
- If we can, then we increment the count.

- Its better to handle the numbers as strings, as we can easily swap the digits.
- We can compare the lengths of the strings and add the preceeding zeros to the smaller string.
- Then parse the strings and check how many different digits are there.
- If there are more than two different digits,
  then we cannot make them equal.
- If there are exactly two different digits,
  then check if they can be swapped to make the numbers equal.
```

### 2. Implementation

```markdown
- Define a `helper` function to check if two numbers are almost equal.

  - Input: two integers `a` and `b`.
    - Convert the integers to strings `sa` and `sb`.
    - Find the maximum length of the strings.
    - Add the preceeding zeros to the smaller string.
    - If the strings are equal, return `true`.
    - Initialize the `diffCount` to `0`.
    - Initialize the `firstDiff` and `secondDiff` to `-1`.
      This will store the indices of the different digits.
    - Iterate over the strings:
      - If the digits are different:
        - Increment the `diffCount`.
        - If the `diffCount` is `1`, store the index in `firstDiff`.
        - If the `diffCount` is `2`, store the index in `secondDiff`.
        - If the `diffCount` is more than `2`, return `false`.
    - Return `true` if the `diffCount` is `2` and if
      `sa[firstDiff] == sb[secondDiff] and sa[secondDiff] == sb[firstDiff]`.

- Define a `countPairs` function to count the almost equal pairs.
  - Input: a vector of integers `nums`.
    - Initialize the `count` to `0`.
    - Initialize the `n` to the size of the `nums`.
    - Iterate over the `nums`:
    - Iterate over the `nums` starting from the next element:
      - If the `helper` function returns `true`, increment the `count`.
    - Return the `count`.
```

> There is a follow-up problem to this problem. [Problem 3267](https://leetcode.com/problems/count-almost-equal-pairs-ii/description/).
