+++
title = 'Problem 3233 Find the Count of Numbers Which Are Not Special'
date = 2024-07-29T19:08:13+05:30
draft = false
series = 'leetcode'
tags =['math','prime-counting']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3233](https://leetcode.com/problems/find-the-count-of-numbers-which-are-not-special/description/)

## Question

You are given 2 positive integers `l` and `r`. For any number `x`, all positive divisors of `x` except `x` are called the proper divisors of `x`.

A number is called special if it has exactly 2 proper divisors. For example:

- The number 4 is special because it has proper divisors 1 and 2.
- The number 6 is not special because it has proper divisors 1, 2, and 3.
- Return the count of numbers in the range `[l, r]` that are not special.

## Example 1

```
Input: l = 5, r = 7

Output: 3

Explanation:

There are no special numbers in the range [5, 7].
```

## Example 2

```
Input: l = 4, r = 16

Output: 11

Explanation:

The special numbers in the range [4, 16] are 4 and 9.
```

## Constraints

```markdown
- `1 <= l <= r <= 10^9`
```

### note

From observation we can understand that all the special numbers are the squares of prime numbers, so the maximum prime number which can satisfy the condition is sqrt(r) and we can find all the prime numbers upto sqrt(r) and then we can find the square of those prime numbers and check if they are in the range [l, r] and count them.

## Solution

```cpp
class Solution {
public:
    int nonSpecialCount(int l, int r) {
        int limit = sqrt(r) + 1;
        vector<bool> isPrime(limit + 1, true);
        isPrime[0] = isPrime[1] = false;

        for (int i = 2; i * i <= limit; ++i) {
            if (isPrime[i]) {
                for (int j = i * i; j <= limit; j += i) {
                    isPrime[j] = false;
                }
            }
        }

        vector<int> primes;
        for (int i = 2; i <= limit; ++i) {
            if (isPrime[i]) {
                primes.push_back(i);
            }
        }

        int splCnt = 0;
        for (int prime : primes) {
            long long square = (long long)prime * prime;
            if (square >= l && square <= r) {
                splCnt++;
            }
        }

        return r-l+1- splCnt;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm             | Time Complexity           | Space Complexity |
| --------------------- | ------------------------- | ---------------- |
| Sieve of Eratosthenes | O(sqrt(r)loglog(sqrt(r))) | O(sqrt(r))       |
| Counting              | O(sqrt(r))                | O(sqrt(r))       |
```

## Explanation

### 1. Intuition

```markdown
- From observation we can understand that all the special numbers are the squares of prime numbers,
  so the maximum prime number which can satisfy the condition is sqrt(r)
  then we can find all the prime numbers upto sqrt(r) and then we can find the square of those prime numbers
  then check if they are in the range [l, r] and count them.
```

### 2. Implementation

```markdown
- We will first find all the prime numbers upto sqrt(r) using the Sieve of Eratosthenes algorithm.
- Then we will find the square of all the prime numbers and check if they are in the range [l, r] and count them.
- Then return `r-l+1- splCnt` which will give us the count of numbers which are not special.
```
