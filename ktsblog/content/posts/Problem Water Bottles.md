+++
title = 'Problem Water Bottles'
date = 2024-07-07T17:59:47+05:30
draft = false
series = 'leetcode'
tags =['math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1518](https://leetcode.com/problems/water-bottles/description/)

## Question

There are `numBottles` water bottles that are initially full of water. You can exchange `bumExchange` empty water bottles for one full water bottle. The operation of drinking a full water bottle turns it into an empty bottle.

Given two integers `numBottles` and `numExchange`, return the maximum number of water bottles you can drink.

## Example 1

```
Input: numBottles = 9, numExchange = 3
Output: 13
Explanation: You can exchange 3 empty bottles to get 1 full water bottle.
Number of water bottles you can drink: 9 + 3 + 1 = 13.
```

## Example 2

```
Input: numBottles = 15, numExchange = 4
Output: 19
Explanation: You can exchange 4 empty bottles to get 1 full water bottle.
Number of water bottles you can drink: 15 + 3 + 1 = 19.
```

## Constraints

```markdown
- `1 <= numBottles <= 100`
- `2 <= numExchange <= 100`
```

## Solution

```cpp
class Solution {
public:
    int numWaterBottles(int numBottles, int numExchange) {
        int numEmpty = 0, res = 0;

        while (numBottles) {
            res += numBottles;
            numEmpty += numBottles;
            numBottles = numEmpty / numExchange;
            numEmpty = numEmpty % numExchange;
        }
        return res;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- We need to calculate the maximum number of water bottles we can drink.
- First assign the number of empty bottles to 0 and the result to 0.
- While the number of bottles is not 0, add the number of bottles to the result.(Simulates drinking the water)
- Add the number of bottles to the number of empty bottles.(Simulates the number of empty bottles generated)
- Calculate the number of bottles that can be exchanged for full bottles and the number of empty bottles left.
- This will be empty bottles/numExchange.
- Calculate the left over empty bottles.
- This will be empty bottles%numExchange.
- Continue the process until the number of bottles is not 0.
```

### 2. Implementation

```markdown
- Assign the number of empty bottles `numEmpty` to 0 and the result `res` to 0.
- While `numBottles` is not 0, do:
  1. Add the `numBottles` to the `res`.
  2. Add the `numBottles` to the `numEmpty`.
  3. Update the `numBottles` to `numEmpty/numExchange`.
     - This simulates the number of bottles that can be exchanged for full bottles.
  4. Update the `numEmpty` to `numEmpty%numExchange`.
     - This simulates the number of empty bottles left after exchanging.
  5. Continue the process until the number of bottles is not 0.
- Return the result.
```

### Mathematical solution

```cpp
class Solution {
public:
    int numWaterBottles(int numBottles, int numExchange) {
        return numBottles + (numBottles - 1) / (numExchange - 1);
    }
};
```

### Explanation

```markdown
- The mathematical solution is derived from the fact that we can drink all the water bottles and exchange the empty bottles for full bottles.
- How many empty bottles can we exchange for full bottles?
  - If we give `numExchange` empty bottles, we can get 1 full bottle.
  - Then its same as giving out `numExchange - 1` empty bottles to get a refill.
  - If we keep aside 1 bottle to fill the refill, we can exchange `numBottles-1`.
  - Hence, the total number of bottles we can drink = `numBottles + (numBottles - 1) / (numExchange - 1)`.
```
