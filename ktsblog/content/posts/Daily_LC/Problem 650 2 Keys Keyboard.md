+++
title = 'Problem 650 2 Keys Keyboard'
date = 2024-08-19T22:02:24+05:30
draft = false
series = 'leetcode'
tags =['math','dynamic-programming']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 650](https://leetcode.com/problems/2-keys-keyboard/description/)

## Question

There is only one character `'A'` on the screen of a notepad. You can perform one of two operations on this notepad for each step:

- Copy All: You can copy all the characters present on the screen (a partial copy is not allowed).
- Paste: You can paste the characters which are copied last time.

Given an integer `n`, return the minimum number of operations to get the character `'A'` exactly `n` times on the screen.

## Example 1

```
Input: n = 3
Output: 3
Explanation: Initially, we have one character 'A'.
In step 1, we use Copy All operation.
In step 2, we use Paste operation to get 'AA'.
In step 3, we use Paste operation to get 'AAA'.
```

## Example 2

```
Input: n = 1
Output: 0
```

## Constraints

```markdown
- `1 <= n <= 1000`
```

## Solution

```cpp
// Recusrive solution
class Solution {
private:
    int targetLength;

    int findMinSteps(int currentLength, int clipboardLength) {
        if (currentLength == targetLength) return 0;
        if (currentLength > targetLength) return INT_MAX / 2;

        int copyAndPaste = 2 + findMinSteps(currentLength * 2, currentLength);
        int pasteOnly = 1 + findMinSteps(currentLength + clipboardLength, clipboardLength);

        return min(copyAndPaste, pasteOnly);
    }

public:
    int minSteps(int n) {
        if (n == 1)
            return 0;
        targetLength = n;
        return 1 + findMinSteps(1, 1);
    }
};
// Using memoization
class Solution {
private:
    int targetLength;
    vector<vector<int>> cache;

    int calculateMinOps(int currentLength, int clipboardLength) {
        if (currentLength == targetLength) return 0;
        if (currentLength > targetLength) return INT_MAX / 2;

        if (cache[currentLength][clipboardLength] != -1) {
            return cache[currentLength][clipboardLength];
        }

        int pasteOption = 1 + calculateMinOps(currentLength + clipboardLength, clipboardLength);
        int copyPasteOption = 2 + calculateMinOps(currentLength * 2, currentLength);

        int result = min(pasteOption, copyPasteOption);
        cache[currentLength][clipboardLength] = result;

        return result;
    }

public:
    int minSteps(int n) {
        if (n == 1) return 0;
        targetLength = n;
        cache = vector<vector<int>>(n + 1, vector<int>(n / 2 + 1, -1));
        return 1 + calculateMinOps(1, 1);
    }
};

// Using factorization

class Solution {
public:
    int minSteps(int n) {
        if (n == 1)
            return 0;
        int sqrtN = sqrt(n);
        int total_operations = 0;
        for (int factor = 2; factor <= sqrtN; factor++) {
            while (n % factor == 0) {
                total_operations += factor;
                n /= factor;
            }
        }
        if (n > 1) {
            total_operations += n;
        }
        return total_operations;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm     | Time Complexity | Space Complexity |
| ------------- | --------------- | ---------------- |
| Memoization   | O(n^2)          | O(n^2)           |
| Factorization | O(sqrt(n))      | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- This is for dynamic programming
- In this method, we can build a recursive function as follows

  - Let `f(x,y)` be the minimum number of operations to get the character 'A' exactly `x` times
    on the screen with `y` characters in the clipboard.
  - We can copy all the characters present on the screen and paste them to get `2x` characters
    on the screen and `x` characters in the clipboard. This operation takes `2` steps.
  - Or we can paste the characters which are copied last time to get `x+y` characters
    on the screen and `y` characters in the clipboard. This operation takes `1` step.
  - The base case will be when `x` is equal to the target length, we return `0`.
  - If not base case then we need to return the minimum of `2 + f(2*x,x)` and `1 + f(x+y,y)`.

- Using the above recursive function we can design a DP solution.
- use a 2D array to store the minimum operations for each `x` and `y`.
- `dp[x][y]` will denote the minimum number of operations to get the character 'A' exactly `x` times
  on the screen with `y` characters in the clipboard.
```

```markdown
- This is for factorization method
- By observation we can say the following things
  1. If n is prime then the minimum number of operations will be n.
     We can build `n` by copying once and pasting `n-1` times.
  2. If `n` is a power of a prime number `p`, then the minimum number of operations will be `p`.
     This is because we can first build `p` by copying once and pasting `p-1` times.
     Then we can build `n` by copying `p` and pasting `n/p-1` times.
  3. If `n` is not a power of a prime number, then we can factorize `n` into prime numbers.
     The minimum number of operations will be the sum of the prime factors.
     This is because we can build `n` by copying the prime factors and pasting the prime factors-1 times.
- We can iterate from `2` to `sqrt(n)` and check if `n` is divisible by `i`.
  While `n` is divisible by `i`, we can add `i` to the total operations and divide `n` by `i`.
- This will not consider non prime factors of `n`.
  Because if `i` is not a prime factor of `n`, then `n/i` will be a prime factor of `n`.
- Finally, we can return the total operations.
```

### 2. Implementation

```markdown
- For memoization, we can use a 2D array to store the minimum operations for each `x` and `y`.
- Declare the `targetLength` and `cache` as variables.
- Write a recursive function `calculateMinOps` to calculate the minimum operations.
- In the recursive function, we can check if the current length is equal to the target length.
- If the current length is greater than the target length, we can return `INT_MAX / 2`.
- If the current length is equal to the target length, we can return `0`.
- If the value of `cache[currentLength][clipboardLength]` is not `-1`, we can return the value.
- Otherwise, we can calculate the minimum operations for the two options.
  - `pasteOption` = 1 + `calculateMinOps(currentLength + clipboardLength, clipboardLength)`
  - `copyPasteOption` = 2 + `calculateMinOps(currentLength * 2, currentLength)`
- `cache[currentLength][clipboardLength]` will be updated with the minimum of the two options.
- Finally, we can return the minimum of the two options.
```

```markdown
- Initialize the variable `total_operations` to `0`.
- Iterate from `2` to `sqrt(n)` and check if `n` is divisible by `i`.
  - while `n` is divisible by `i`, add `i` to the `total_operations` and divide `n` by `i`.
- If `n` is greater than `1`, add `n` to the `total_operations`.
- This will handle the case where `n` is itself a prime number.
- Finally, return the `total_operations`.
```
