+++
title = 'Problem 3259 Maximum Energy Boost From Two Drinks'
date = 2024-08-19T11:21:48+05:30
draft = false
series = 'leetcode'
tags =['dynamic-programming']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3259](https://leetcode.com/problems/maximum-energy-boost-from-two-drinks/description/)

## Question

You are given two integer arrays `energyDrinkA` and `energyDrinkB` of the same length `n` by a futuristic sports scientist. These arrays represent the energy boosts per hour provided by two different energy drinks, A and B, respectively.

You want to maximize your total energy boost by drinking one energy drink per hour. However, if you want to switch from consuming one energy drink to the other, you need to wait for one hour to cleanse your system (meaning you won't get any energy boost in that hour).

Return the maximum total energy boost you can gain in the next `n` hours.

Note that you can start consuming either of the two energy drinks.

## Example 1

```
Input: energyDrinkA = [1,3,1], energyDrinkB = [3,1,1]

Output: 5

Explanation:

To gain an energy boost of 5, drink only the energy drink A (or only B).
```

## Example 2

```
Input: energyDrinkA = [4,1,1], energyDrinkB = [1,1,3]

Output: 7

Explanation:

To gain an energy boost of 7:

Drink the energy drink A for the first hour.
Switch to the energy drink B and we lose the energy boost of the second hour.
Gain the energy boost of the drink B in the third hour.
```

## Constraints

```markdown
- `n == energyDrinkA.length == energyDrinkB.length`
- `3 <= n <= 10^5`
- `1 <= energyDrinkA[i], energyDrinkB[i] <= 10^5`
```

## Solution

```cpp
// Memoization
class Solution {
public:
    long long helper(vector<int>& a, vector<int>& b, int i, int k, vector<vector<long long>>&dp){
        if(i == a.size()){
            return 0;
        }
        if(dp[k][i] != -1)
            return dp[k][i];
        long long take = 0, nottake = 0;
        if(k == 1){
            take = a[i] + helper(a,b,i+1,k,dp);
            nottake = helper(a,b,i+1,2,dp);
        }
        else if(k == 2){
            take = b[i] + helper(a,b,i+1,k,dp);
            nottake = helper(a,b,i+1,1,dp);
        }
        return dp[k][i] = max(take,nottake);
    }
    long long maxEnergyBoost(vector<int>& a, vector<int>& b) {
        vector<vector<long long>>dp(3 , vector<long long>(a.size() , -1));
        return max(helper(a,b,0,1,dp) , helper(a,b,0,2,dp));
    }
};

// Tabulation
class Solution {
public:

    long long maxEnergyBoost(vector<int>& A, vector<int>& B) {
        int n = A.size();
        vector<vector<long long>> dp(n + 5, vector<long long>(2, 0));

        for(int i = n - 1; i >= 0; i--){
            dp[i][0] = max(B[i] + dp[i + 1][0], dp[i + 1][1]);
            dp[i][1] = max(A[i] + dp[i + 1][1], dp[i + 1][0]);
        }

        return max(dp[0][0], dp[0][1]);
    }
};

// Optimized Tabulation
class Solution {
public:
    long long maxEnergyBoost(std::vector<int>& energyDrinkA, std::vector<int>& energyDrinkB) {
        int n = energyDrinkA.size();
        long long dpA = energyDrinkA[0], dpB = energyDrinkB[0];

        for (int i = 1; i < n; ++i) {
            long long newDpA = max(dpA + energyDrinkA[i], dpB);
            long long newDpB = max(dpB + energyDrinkB[i], dpA);
            dpA = newDpA;
            dpB = newDpB;
        }

        return max(dpA, dpB);
    }
};
```

## Complexity Analysis

```markdown
| Algorithm            | Time Complexity | Space Complexity |
| -------------------- | --------------- | ---------------- |
| Memoization          | O(n)            | O(n)             |
| Tabulation           | O(n)            | O(n)             |
| Optimized Tabulation | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- We can start from either of the two energy drinks.
- Since the condition says we need to give an hour gap to switch between drinks, we can keep track of the maximum energy boost we can get from each drink.
- We can use dynamic programming to solve this problem.
- Two variables `dpA` and `dpB` can be used to keep track of the maximum energy boost we can get from each drink.
- Initially `dpA` and `dpB` will be the energy boost from the first hour of each drink.
- From the 2nd hour, the maximum boost we can get is
  - Option 1 - Drink the same drink as the previous hour.
  - Option 2 - Switch to the other drink.
  - Now find the maximum of the two options.
  - This will be new dpA and dpB.
- We can update `dpA` and `dpB` with the new values.
- Finally, we can return the maximum of `dpA` and `dpB`.
```

### 2. Implementation

```markdown
- Intialize two variables `dpA` and `dpB` to store the maximum energy boost we can get from each drink.
- Iterate over the arrays `energyDrinkA` and `energyDrinkB` from the second element.
- For `i` from `1` to `n-1` do
  - Calculate the new values of `dpA` and `dpB` using the formula
    - `newDpA = max(dpA + energyDrinkA[i], dpB)`
    - `newDpB = max(dpB + energyDrinkB[i], dpA)`
  - Update `dpA` and `dpB` with the new values.
- Finally, return the maximum of `dpA` and `dpB`.
```

> This problem is a variation of the house robber problem. We can solve this problem using dynamic programming. We can use memoization, tabulation, or optimized tabulation to solve this problem. The optimized tabulation approach is the most efficient approach to solve this problem
