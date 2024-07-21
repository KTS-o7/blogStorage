+++
title = 'Problem 633 Sum of Square Numbers'
date = 2024-06-17T18:55:17+05:30
draft = false
series = 'leetcode'
tags =['two-pointer','math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 633](https://leetcode.com/problems/sum-of-square-numbers/description/)

## Question

Given a non-negative integer `c`, decide whether there are two integers `a` and `b` such that `a^2 + b^2 = c`.

### Note

- Even thought it doesn't say in the question , the numbers can be equal, ie `a=b` is valid.
- since we are talking about square sums, it is safe to say that values of any one of variable can be atmost `sqrt(c)`

## Example 1

```text
Input: c = 5
Output: true
Explanation: 1 * 1 + 2 * 2 = 5
```

## Example 2

```text
Input: c = 3
Output: false
```

## Constraints

- `0 <= c <= 2^31 - 1`

## Solution

```cpp
class Solution {
public:
    bool judgeSquareSum(int c) {
        long long int l =0,r = sqrt(c);
        long long int sum;
        while(l<=r)
        {
            sum = l*l + r*r;
            if(sum == c)
                return true;
            else if(sum>c)
                r--;
            else
                l++;
        }
        return false;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(sqrt(n))
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- The equation `a^2 + b^2 = c` can be resolved into a two pointer problem.
- We know that one of the values of `a` and `b` can be atmost `sqrt(c)`.
- When `l` and `r` are at the extreme ends, the sum of squares will be maximum.
- So we can start from `l=0` and `r=sqrt(c)` and keep moving the pointers based on the sum.
- If the sum is greater than `c`, we need to reduce the sum, so we decrement `r`.
- If the sum is lesser than `c`, we need to increase the sum, so we increment `l`.
- If the sum is equal to `c`, we return true.
- If the pointers cross each other, we return false.
```

### 2. Implementation

```markdown
- Initialize `long long int l =0,r = sqrt(c);` and `long long int sum;`.
- Run a while loop till `l<=r`.
- Calculate the sum of squares `sum = l*l + r*r`.
- If the sum is equal to `c`, return true.
- If the sum is greater than `c`, decrement `r`.
- If the sum is lesser than `c`, increment `l`.
- If the loop ends, return false.
```

### Mathematical Solution

- There is a pure mathematical solution to this problem.

#### Fermat's Theorem on Sum of Two Squares

```markdown
Any positive number `n` is expressible as a sum of two squares
if and only if the prime factorization of `n`,
every prime of the form `(4k+3)` occurs an even number of times.
```

- Using the above theorem we can check if any of the prime factors of `c` which is of the form `(4k+3)` occurs an odd number of times.
- If yes, then we can return false.
- If no, then we need to check if the number `c` itself is of form `(4k+3)`.
- If yes, then we can return false.
- If no, then we can return true.

### Code

```cpp
public class Solution {
    public boolean judgeSquareSum(int c) {
        for (int i = 2; i * i <= c; i++) {
            int count = 0;
            if (c % i == 0) {
                while (c % i == 0) {
                    count++;
                    c /= i;
                }
                if (i % 4 == 3 && count % 2 != 0)
                    return false;
            }
        }
        return c % 4 != 3;
    }
}
```

#### Complexity

- `Time Complexity` : O(sqrt(n))
- `Space Complexity` : O(1)

> The wikipedia article for this theorem is [here](https://en.wikipedia.org/wiki/Fermat%27s_theorem_on_sums_of_two_squares)
