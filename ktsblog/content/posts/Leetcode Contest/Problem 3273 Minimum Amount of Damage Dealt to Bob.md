+++
title = 'Problem 3273 Minimum Amount of Damage Dealt to Bob'
date = 2024-09-01T16:34:05+05:30
draft = false
series = 'leetcode'
tags =['sorting','custom-comparator','counting']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3273](https://leetcode.com/problems/minimum-amount-of-damage-dealt-to-bob/description/)

## Question

You are given an integer `power` and two integer arrays `damage` and `health`, both having length `n`.

Bob has `n` enemies, where enemy `i` will deal Bob `damage[i]` points of damage per second while they are alive (i.e. `health[i] > 0`).

Every second, after the enemies deal damage to Bob, he chooses one of the enemies that is still alive and deals `power` points of damage to them.

Determine the minimum total amount of damage points that will be dealt to Bob before all `n` enemies are dead.

## Example 1

```
Input: power = 4, damage = [1,2,3,4], health = [4,5,6,8]

Output: 39

Explanation:

Attack enemy 3 in the first two seconds, after which enemy `3` will go down,
the number of damage points dealt to Bob is `10 + 10 = 20` points.
Attack enemy 2 in the next two seconds, after which enemy `2` will go down,
the number of damage points dealt to Bob is `6 + 6 = 12` points.
Attack enemy 0 in the next second, after which enemy `0` will go down,
the number of damage points dealt to Bob is `3` points.
Attack enemy `1` in the next two seconds, after which enemy `1` will go down,
the number of damage points dealt to Bob is `2 + 2 = 4` points.
```

## Example 2

```
placeHoldeInput: power = 1, damage = [1,1,1,1], health = [1,2,3,4]

Output: 20

Explanation:

Attack enemy 0 in the first second, after which enemy 0 will go down,
the number of damage points dealt to Bob is 4 points.
Attack enemy 1 in the next two seconds, after which enemy 1 will go down,
the number of damage points dealt to Bob is 3 + 3 = 6 points.
Attack enemy 2 in the next three seconds, after which enemy 2 will go down,
the number of damage points dealt to Bob is 2 + 2 + 2 = 6 points.
Attack enemy 3 in the next four seconds, after which enemy 3 will go down,
the number of damage points dealt to Bob is 1 + 1 + 1 + 1 = 4 points.
```

## Example 3

```
Input: power = 8, damage = [40], health = [59]

Output: 320
```

## Edge Case

```
Input
power = 28
damage = [95,94]
health = [23,2]
Output
283
```

```
Input
power = 62
damage = [80,79]
health = [86,13]
Output
319
```

## Constraints

- 1 <= `power` <= 10<sup>4</sup>
- 1 <= n == `damage.length` == `health.length` <= 10<sup>5</sup>
- 1 <= `damage[i], health[i]` <= 10<sup>4</sup>

## Solution

```cpp
class Solution {
public:
    long long minDamage(int power, vector<int>& damage, vector<int>& health) {
        int n = damage.size();
        long long totdmg = 0;
        long long res = 0;

        vector<tuple<double, int, int>> e;

        for (int i = 0; i < n; i++) {
            int turns = (health[i] + power - 1) / power;
            double eff = static_cast<double>(damage[i]) / turns;
            e.push_back({eff, damage[i], turns});
            totdmg += damage[i];
        }

        sort(e.begin(), e.end(), [](const auto& a, const auto& b) {
            if (get<0>(a) != get<0>(b))
                return get<0>(a) > get<0>(b);
            return get<1>(a) > get<1>(b);
        });

        for (const auto& [eff, dmg, turns] : e) {
            res += totdmg * turns;
            totdmg -= dmg;
        }

        return res;
    }
};

```

## Complexity Analysis

```markdown
| Algorithm      | Time Complexity | Space Complexity |
| -------------- | --------------- | ---------------- |
| Tuple creation | O(N)            | O(N)             |
| Sorting        | O(NlogN)        | O(1)             |
| Total          | O(NlogN)        | O(N)             |
```

## Explanation

### 1. Intuition

We have different cases in our hand.
To minimize the damage, we need to pay attention to both damage dealt by enemies and the health of the enemies.

- An enemy can deal lot of damage if it has high damage or low health.
- An enemy can deal less damage if it has low damage and low health.
- An enemy can deal lot of damage if it has high damage and high health.
- An enemy can deal lot of damage if it has low damage and high health.

Hence we need to calculate the effective damage dealt by each enemy and sort them in descending order.

- Order of killing will be as follows
  - If the enemy has high effective damage, kill it first.
  - If two enemies has same effective damage, kill the one with high damage first.

```markdown
- First count the number of turns needed to kill an enemy using the formula (health[i] + power - 1) / power.
- This is counted using ceil division.
- Calculate the effective damage dealt by each enemy using the formula damage[i] / turns.
- Store the effective damage, damage and turns in a tuple.
- Sort the enemies in descending order of effective damage.
- Calculate the total damage dealt by all enemies.
- For each enemy, calculate the total damage dealt by all enemies and add it to the result.
- Update the total damage dealt by all enemies by subtracting the damage dealt by the current enemy. Indicating that the enemy is dead.
```

### 2. Implementation

```markdown
- Declare a variable `n` to store the length of the damage array.
- Declare a variable `totdmg` to store the total damage dealt by all enemies.
- Declare a variable `res` to store the result.

- Declare a vector of tuples `e` to store the effective damage, damage and turns needed to kill each enemy.

- Iterate over from `0` to `n`.

  - Declare a variable `turns` to store the number of turns needed to kill the enemy.
  - Calculate the number of turns needed to kill the enemy using the formula `(health[i] + power - 1) / power`.
  - Calculate the effective damage dealt by the enemy using the formula `static_cast<double>(damage[i]) / turns`.
  - Push the tuple `{eff, damage[i], turns}` to the vector `e`.
  - Add the damage dealt by the enemy to the total damage dealt by all enemies.

- Sort the vector `e` in descending order of effective damage.
- Custom comparator is as follows

  - If the effective damage of the first enemy is not equal to the effective damage of the second enemy, sort in descending order of effective damage.
  - If the effective damage of the first enemy is equal to the effective damage of the second enemy, sort in descending order of damage.

- For each enemy in the vector `e`, calculate the total damage dealt by all enemies and add it to the result.
- Update the total damage dealt by all enemies by subtracting the damage dealt by the current enemy. Indicating that the enemy is dead.

- Return the result.
```
