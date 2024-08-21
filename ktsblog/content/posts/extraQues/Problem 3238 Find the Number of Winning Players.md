+++
title = 'Problem 3238 Find the Number of Winning Players'
date = 2024-08-21T22:42:42+05:30
draft = false
series = 'leetcode'
tags =['matrix','hash-map','simulation']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3238](https://leetcode.com/problems/find-the-number-of-winning-players/description/)

## Question

You are given an integer `n` representing the number of players in a game and a 2D array `pick` where `pick[i] = [xi, yi]` represents that the player `xi` picked a ball of color `yi`.

Player `i` wins the game if they pick strictly more than `i` balls of the same color. In other words,

- Player `0` wins if they pick any ball.
- Player `1` wins if they pick at least two balls of the same color.
- Player `i` wins if they pick at least `i + 1` balls of the same color.
- Return the number of players who win the game.

#### Note that multiple players can win the game.

## Example 1

```
Input: n = 4, pick = [[0,0],[1,0],[1,0],[2,1],[2,1],[2,0]]

Output: 2

Explanation:

Player 0 and player 1 win the game, while players 2 and 3 do not win.
```

## Example 2

```
Input: n = 5, pick = [[1,1],[1,2],[1,3],[1,4]]

Output: 0

Explanation:

No player wins the game.
```

## Example 3

```
Input: n = 5, pick = [[1,1],[2,4],[2,4],[2,4]]

Output: 1

Explanation:

Player 2 wins the game by picking 3 balls with color 4.
```

## Constraints

```markdown
- `2 <= n <= 10`
- `1 <= pick.length <= 100`
- `pick[i].length == 2`
- `0 <= xi <= n - 1`
- `0 <= yi <= 10`
```

## Solution

```cpp
class Solution {
public:
    int winningPlayerCount(int n, vector<vector<int>>& pick) {
        vector<vector<int>>cnt(n,vector<int>(11,0));
        for(auto &it:pick){
            cnt[it[0]][it[1]]++;
        }
        int ans = 0;
        for(int i=0;i<n;i++){
            for(int j=0;j<11;j++){
                //cout<<cnt[i][j]<<" ";
                if(cnt[i][j]>i){
                    ans++;
                    break;
                }
            }
            //cout<<endl;
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Traversal | O(10n)          | O(10n)           |
```

## Explanation

### 1. Intuition

```markdown
- We need to keep track of two things
  1.  How many balls are picked up by player `xi`.
  2.  How many balls of color `yi` are picked up .
- This can be modelled as a 2D array `cnt`
  where `cnt[i][j]` represents the number of balls of color `j` picked up by player `i`.
- Iterate over the `pick` array and update the `cnt` array.
- Iterate over the `cnt` array and check if the player has picked up more than `i` balls of the same color.
- If yes, increment the answer.
- Return the answer.
```

### 2. Implementation

```markdown
- Initialize a 2D array `cnt` of size `n x 11` with all elements as `0`.
- Iterate over the `pick` array and update the `cnt` array.
- Initialize a variable `ans` to `0`.
- For each player `i` from `0` to `n-1`,
  - For each color `j` from `0` to `10`,
    - If the player has picked up more than `i` balls of the same color,
      - Increment the answer.
      - Break the inner loop. // to avoid double counting.
- Return the answer.
```

