+++
title = 'Problem 264 Ugly Number II'
date = 2024-08-24T15:04:39+05:30
draft = false
series = 'leetcode'
tags =['math','min-heap','priority-queue']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 264](https://leetcode.com/problems/ugly-number-ii/description/)

## Question

An ugly number is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return the `nth` ugly number.

## Example 1

```
Input: n = 10
Output: 12
Explanation: [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.
```

## Example 2

```
Input: n = 1
Output: 1
Explanation: 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.
```

## Constraints

```markdown
- `1 <= n <= 1690`
```

## Solution

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> primes = {2, 3, 5};
        priority_queue<long, vector<long>, greater<long>> uglyHeap;
        unordered_set<long> visited;

        uglyHeap.push(1);
        visited.insert(1);

        long curr;
        for (int i = 0; i < n; ++i) {
            curr = uglyHeap.top();
            uglyHeap.pop();
            for (int prime : primes) {
                long new_ugly = curr * prime;
                if (visited.find(new_ugly) == visited.end()) {
                    uglyHeap.push(new_ugly);
                    visited.insert(new_ugly);
                }
            }
        }
        return (int)curr;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Min Heap  | O(nlogn)        | O(n)             |
```

## Explanation

### 1. Intuition

```markdown
- There is a crude approach to solve this by generating the ugly numbers,
  till we reach the `n`th ugly number.
- Since the given constraints say that each number's prime factors are limited to `2`, `3`, and `5`,
  we can generate the ugly numbers by multiplying the previous ugly number with `2`, `3`, and `5`.
- Each time we need to find the minimum ugly number then generate the next set of ugly numbers.

- So lets have a min heap to store the ugly numbers.
- To keep track of ugly numbers already generated, we can use a set.
  this prevents us from generating the same ugly number multiple times.
- We can start with the first ugly number `1` and push it to the heap.
- Then we can generate the next ugly numbers by multiplying the current ugly number with `2`, `3`, and `5`.
- Push the new ugly numbers to the heap and keep track of them in the set.
- We can repeat this process till we reach the `n`th ugly number.
```

### 2. Implementation

```markdown
- Initialize a vector of primes `{2, 3, 5}`.
- Initialize a min heap `uglyHeap` of type `long` and a set `visited` of type `long`.
- Push the first ugly number `1` to the heap and insert it into the set.
- Initialize a variable `curr` to store the current ugly number.
- Iterate from `0` to `n`:
  - Get the top element from the heap and store it in `curr`.
  - Pop the top element from the heap.
  - Iterate over the primes:
    - Multiply the current ugly number with the prime and store it in `new_ugly`.
    - If the new ugly number is not present in the set:
      - Push the new ugly number to the heap.
      - Insert the new ugly number into the set.
- Return the current ugly number.
```

> There is also a dynamic programming approach to solve this problem.

### DP Solution
```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> primes = {2, 3, 5};  // Initialize the primes array
        vector<int> indices = {0, 0, 0}; // Initialize indices for multiples of 2, 3, 5
        vector<int> uglyArr(1, 1);       // Initialize the ugly number array with 1

        for (int i = 1; i < n; ++i) {
            // Calculate the next possible ugly numbers
            vector<int> next_uglies = {
                uglyArr[indices[0]] * primes[0],
                uglyArr[indices[1]] * primes[1],
                uglyArr[indices[2]] * primes[2]
            };
            int min_value = *min_element(next_uglies.begin(), next_uglies.end()); // Find the smallest value
            uglyArr.push_back(min_value); // Append the smallest value to uglyArr

            // Update indices for primes that generated the current min_value
            for (int j = 0; j < 3; ++j) {
                if (next_uglies[j] == min_value) {
                    ++indices[j];
                }
            }
        }

        return uglyArr[n - 1];
    }
};
```

### Explanation

The above code generates the `n`th ugly number using dynamic programming.

```markdown
- We initialize the primes array with `2`, `3`, and `5`.
- We also initialize the indices array with `0`, `0`, and `0`
  to keep track of the multiples of `2`, `3`, and `5`.
- Indicies array will keep track of the next ugly number to be generated for each prime.
- We initialize the ugly number array with `1`.
- We iterate from `1` to `n`:
  - Calculate the next possible ugly numbers by
    multiplying the current ugly numbers with the primes.
  - Find the smallest value from the next possible ugly numbers.
  - Append the smallest value to the ugly number array.
  - Update the indices for the primes that generated the current smallest value.
- Return the `n`th ugly number.
```
