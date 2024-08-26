+++
title = 'Problem 3232 Find if Digit Game Can Be Won'
date = 2024-07-29T19:00:51+05:30
draft = false
series = 'leetcode'
tags =['math','counting']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3232](https://leetcode.com/problems/find-if-digit-game-can-be-won/description/)

## Question

You are given an array of positive integers `nums`.

Alice and Bob are playing a game. In the game, Alice can choose either all single-digit numbers or all double-digit numbers from `nums`, and the rest of the numbers are given to Bob. Alice wins if the sum of her numbers is strictly greater than the sum of Bob's numbers.

Return `true` if Alice can win this game, otherwise, return `false`.

## Example 1

```
Input: nums = [1,2,3,4,10]

Output: false

Explanation:

Alice cannot win by choosing either single-digit
or double-digit numbers.
```

## Example 2

```
Input: nums = [1,2,3,4,5,14]

Output: true

Explanation:

Alice can win by choosing single-digit numbers which have a sum equal to 15.
```

## Example 3

```
Input: nums = [5,5,5,25]

Output: true

Explanation:

Alice can win by choosing double-digit numbers which have a sum equal to 25.
```

## Constraints

```markdown
- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 99`
```

## Solution

```cpp
class Solution {
public:
    bool canAliceWin(vector<int>& nums) {
        //sort(nums.begin(),nums.end());
        int ones=0,tens=0;
        for(int it:nums){
            if(it<=9)
                ones+=it;
            else
                tens+=it;
        }
        if(ones==tens)
            return false;
        return true;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Traversal | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- By observation we can say that
  1. If the sum of all single digit numbers exceed the sum of all double digit numbers then Alice will choose the set of single digit numbers.
  2. If the sum of all double digit numbers exceed the sum of all single digit numbers then Alice will choose the set of double digit numbers.
- The only case when Alice cannot win is when the sum of all single digit numbers is equal to the sum of all double digit numbers.
```

### 2. Implementation

```markdown
- We will iterate over the array and count the sum of all single digit numbers and double digit numbers.
- If the sum of all single digit numbers is equal to the sum of all double digit numbers then return false.
- Else return true.
```
