+++
title = 'Problem 3290 Maximum Multiplication Score'
date = 2024-09-17T11:40:37+05:30
draft = false
series = 'leetcode'
tags =['dp','dynamic-programming','math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3290](https://leetcode.com/problems/maximum-multiplication-score/description/)

## Question

You are given an integer array `a` of size 4 and another integer array `b` of size at least 4.

You need to choose 4 indices `i0`, `i1`, `i2`, and `i3` from the array `b` such that `i0 < i1 < i2 < i3`. Your score will be equal to the value `a[0] * b[i0] + a[1] * b[i1] + a[2] * b[i2] + a[3] * b[i3]`.

Return the maximum score you can achieve.

## Example 1

```
Input: a = [3,2,5,6], b = [2,-6,4,-5,-3,2,-7]

Output: 26

Explanation:
We can choose the indices 0, 1, 2, and 5.
The score will be 3 * 2 + 2 * (-6) + 5 * 4 + 6 * 2 = 26.
```

## Example 2

```
Input: a = [-1,4,5,-2], b = [-5,-1,-3,-2,-4]

Output: -1

Explanation:
We can choose the indices 0, 1, 3, and 4.
The score will be (-1) * (-5) + 4 * (-1) + 5 * (-2) + (-2) * (-4) = -1.
```

## Constraints

- `a.length` == 4
- 4 <= `b.length` <= 10<sup>5</sup>
- 10<sup>5</sup> <= `a[i], b[i]` <= 10<sup>5</sup>

## Solution

```cpp
// Recursive Solution on Take-Not-Take pattern
class Solution {
public:
    long long maxScore(vector<int>& a, vector<int>& b) {
        return helper(a, b, 0, 0);
    }
    long long helper(vector<int>& a, vector<int>& b, int capacity, int index) {
        if (capacity == 4)
            return 0;
        if (index == b.size())
            return INT_MIN;
        long long take = (long long)a[capacity] * (long long)b[index] + helper(a, b, capacity + 1, index + 1);
        long long notTake = helper(a, b, capacity, index + 1);
        return max(take, notTake);
    }
};

// Memoization Solution
class Solution {
public:
    long long maxScore(vector<int>& a, vector<int>& b) {
        vector<vector<long long>> dp(4, vector<long long>(b.size(), -1));
        return helper(a, b, 0, 0, dp);
    }
    long long helper(vector<int>& a, vector<int>& b, int capacity, int index, vector<vector<long long>>& dp) {
        if (capacity == 4)
            return 0;
        if (index == b.size())
            return -1e11;
        if (dp[capacity][index] != -1)
            return dp[capacity][index];

        long long take = (long long)a[capacity] * (long long)b[index] + helper(a, b, capacity + 1, index + 1, dp);
        long long notTake = helper(a, b, capacity, index + 1, dp);

        return dp[capacity][index] = max(take, notTake);
    }
};

// Tabulation Solution
class Solution {
public:
    long long maxScore(vector<int>& a, vector<int>& b) {
        vector<long long> dp(4, LLONG_MIN);

        for (int index = 0; index < b.size(); ++index) {
            for (int capacity = 3; capacity >= 0; --capacity) {
                if (capacity == 0) {
                    dp[0] = max(dp[0], (long long)a[0] * b[index]);
                }
                else if (dp[capacity - 1] != LLONG_MIN) {
                    dp[capacity] = max(dp[capacity], dp[capacity - 1] + (long long)a[capacity] * b[index]);
                }
            }
        }
        return dp[3];
    }
};

```

## Complexity Analysis

```markdown
| Algorithm   | Time Complexity | Space Complexity |
| ----------- | --------------- | ---------------- |
| Recursive   | O(2^n)          | O(n)             |
| Memoization | O(n^2)          | O(n^2)           |
| Tabulation  | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition

#### Recursive Solution

- This is a classic Take - Not Take pattern problem.
- Since we have to choose 4 indices such that `i0 < i1 < i2 < i3`, we can keep track of the capacity and index.
- If the capacity is 4, we have reached the end of the array and we can return 0.
- If the index is equal to the size of the array, we can return `INT_MIN`.
- So, we can build the solution by considering
  - `Take` - If we include the current index then the score will be `a[capacity] * b[index] + helper(a, b, capacity + 1, index + 1)`.
  - It means we have included the current index and we have to move to the next capacity and index.
  - `Not Take` - If we do not include the current index then the score will be `helper(a, b, capacity, index + 1)`.
- Now we have to return the maximum of `Take` and `Not Take`.

#### Memoization Solution

- Using the above recursive solution, we can memoize the results.
- We can create a 2D vector `dp` of size `4 x b.size()` and initialize it with -1.
- If the value is not -1, we can return the value.
- Else we can store the result in the `dp` and return the result.

#### Tabulation Solution

This is a tricky solution to implement. Somehow we have to consider all the possible combinations of 4 indices and calculate the maximum score.

- Ultimately we know the score of `3`rd element will be maximum , if `0,1,2` scores are maximum and `3`rd element is multiplied with a bigger number.
- Similarly, `2`nd element will be maximum if `0,1` scores are maximum and `2`nd element is multiplied with a bigger number.
- So that means first our consideration will begin from `0`th element and we will keep track of the maximum score of `0,1,2,3` elements.

- Iterate over the array from `0` to `n-1`.
- Iterate over the capacity from `3` to `0`. This is simulating the `Take` and `Not Take` pattern by considering the maximum score of the previous capacity.
- If the capacity is `0`, then we can directly calculate the score by multiplying the `0`th element of `a` with the current element of `b`.
- Else, we can calculate the score by adding the previous capacity score with the current element of `a` multiplied by the current element of `b`.
- So the formula will be `dp[capacity] = max(dp[capacity], dp[capacity - 1] + (long long)a[capacity] * b[index])` when the capacity is not `0`.
- Finally, we can return the `3`rd element of the `dp` array.
- This will give us the maximum score.

### 2. Implementation

```markdown
- Intialize a vector `dp` of size `4` with `LLONG_MIN`.
- Iterate over the array from `0` to `n-1`.
  - Iterate over the capacity from `3` to `0`.
    - If the capacity is `0`, then calculate the score by multiplying the `0`th element of `a` with the current element of `b`.
    - Store it in `dp[0]`.
    - Else if the previous capacity score is not `LLONG_MIN`,
      then calculate the score by
      adding the previous capacity score with the current element of `a` multiplied by the current element of `b`.
    - That is `dp[capacity] = max(dp[capacity], dp[capacity - 1] + (long long)a[capacity] * b[index])`.
- Finally, return the `3`rd element of the `dp` array.
```

> This problem is similar to the [Best time to buy and sell stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)
