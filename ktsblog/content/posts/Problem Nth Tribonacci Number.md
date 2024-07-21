+++
title = 'Problem Nth Tribonacci Number'
date = 2024-04-24T13:45:56+05:30
draft = false
toc = false
series = 'leetcode'
tags = ['cpp','math','dp']
+++

# Problem Statement

**Link** - [Problem 1137](https://leetcode.com/problems/n-th-tribonacci-number/description/)

## Question

The Tribonacci sequence `T(n)` is defined as follows:

`T(0) = 0`, `T(1) = 1`, `T(2) = 1`, and `T(n+3) = T(n) + T(n+1) + T(n+2)` for `n` >= 0.

Given `n`, return the value of `T(n)`.

### Note to self

- This is a straight forward Recursion Problem.
- We can convert this to DP by using memoization.
- We will use an array to store the intermediate values to prevent repeated calculations.

## Example 1

```text
Input: n = 4
Output: 4
Explanation:
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
```

## Example 2

```text
Input: n = 25
Output: 1389537
```

## Constraints

- `0 <= n <= 37`
- The answer is guaranteed to fit within a 32-bit integer, ie. `answer <= 2^31 - 1`.

## Solution

```cpp
class Solution {
public:
    int tribonacci(int n) {
        if(!n)
            return 0;
        if(n==1)
            return 1;
        if(n==2)
            return 1;
        vector<int>dp(n+1);
        dp[0]= 0;
        dp[1] = 1;
        dp[2] = 1;
        for(int i=3;i<=n;i++)
            dp[i]= dp[i-1]+dp[i-2]+dp[i-3];
        return dp[n];
    }
};
```

## Complexity

- `Time: O(n)`
- `Space: O(n)`

## Explaination

1. We will use the DP array to store the intermediate values.
2. We will store 0,1,1 as first 3 values.
3. Then inside a loop from 3 to `n`, we will calculate the value of `T(n)` using the formula `T(n) = T(n-1) + T(n-2) + T(n-3)`.
4. We will return the value of `T(n)` at the end.

### DP without using array

- `It is still memoization`

```cpp
    class Solution {
    public:
        int tribonacci(int n) {
            if(!n)
                return 0;
            if(n==1)
                return 1;
            if(n==2)
                return 1;
            int a=0,b=1,c=1;
            for(int i=3;i<=n;i++)
            {
                int temp = a+b+c;
                a=b;
                b=c;
                c=temp;
            }
            return c;
        }
    };
```

- `Time: O(n)`
- `Space: O(1)`

- We can also solve this using recursion but it will be slow as it will have a time complexity of `O(3^n)`.
- We can also solve this using matrix exponentiation but it will be an overkill for this problem.

---

> This is a demonstration of memoization technique used in DP. With this we can convert a recursive method into dynamic programming method.
