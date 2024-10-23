+++
title = 'Problem 1405 Longest Happy String'
date = 2024-10-23T22:27:57+05:30
draft = false
series = 'leetcode'
tags =[]
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1405]()

## Question

A string `s` is called happy if it satisfies the following conditions:

- `s` only contains the letters `'a'`, `'b'`, and `'c'`.
- `s` does not contain any of `"aaa"`, `"bbb"`, or `"ccc"` as a substring.
- `s` contains at most `a` occurrences of the letter `'a'`.
- `s` contains at most `b` occurrences of the letter `'b'`.
- `s` contains at most `c` occurrences of the letter `'c'`.
  Given three integers `a`, `b`, and `c`, return the longest possible happy string. If there are multiple longest happy strings, return any of them. If there is no such string, return the empty string `""`.

A substring is a contiguous sequence of characters within a string.

## Example 1

```
Input: a = 1, b = 1, c = 7
Output: "ccaccbcc"
Explanation: "ccbccacc" would also be a correct answer.

```

## Example 2

```
Input: a = 7, b = 1, c = 0
Output: "aabaa"
Explanation: It is the only correct answer in this case.

```

## Constraints

- `0 <= a, b, c <= 100`
- `a + b + c > 0`

## Solution

```cpp
class Solution {
public:
    string longestDiverseString(int a, int b, int c) {
        int streak_a = 0, streak_b = 0, streak_c = 0;
        int iter = a + b + c;
        string ans = "";

        for (int i = 0; i < iter; i++) {
            if ((a >= b && a >= c && streak_a != 2) || (a > 0 && (streak_b == 2 || streak_c == 2))) {

                ans += 'a';
                a--;
                streak_a++;
                streak_b = 0;
                streak_c = 0;
            }
            else if ((b >= a && b >= c && streak_b != 2) || (b > 0 && (streak_c == 2 || streak_a == 2))) {

                ans += 'b';
                b--;
                streak_b++;
                streak_a = 0;
                streak_c = 0;
            }
            else if ((c >= a && c >= b && streak_c != 2) || (c > 0 && (streak_a == 2 || streak_b == 2))) {
                ans += 'c';
                c--;
                streak_c++;
                streak_a = 0;
                streak_b = 0;
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Greedy    | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition: The Joy of Being Happy (String)!

Imagine you're a builder with three types of blocks (a, b, and c), and you need to build the longest possible tower with two crucial rules:

- You can't put three identical blocks in a row (that would make the tower unstable!)
- You have a limited supply of each block type (a, b, and c blocks)

This is exactly what our "Happy String" problem is about! But why does a greedy approach work here?

**The Mathematical Beauty**

Let's prove why being greedy (always choosing the most abundant available character that doesn't break the rules) leads to the optimal solution:

1. **Proof by Contradiction:**

   - Assume there exists a longer valid string S' than our greedy solution S
   - At the first position where S and S' differ, S' must choose a less abundant character
   - This means S' is "saving" a more abundant character for later use
   - However, if it was safe to use the more abundant character at this position (doesn't create "aaa", "bbb", or "ccc"):
     - Using it would never prevent us from using other characters later
     - Because we can always use less abundant characters whenever we have two consecutive occurrences of any character
   - Therefore, there's no advantage in "saving" more abundant characters
   - This contradicts our assumption that S' is longer than S

2. **Why Local Optimality Leads to Global Optimality:**
   - At each step, we have at most 3 choices
   - By choosing the most abundant character (when valid):
     - We maximize the usage of the character that has the most potential for length
     - We preserve the ability to use other characters as separators
     - We ensure we don't "waste" positions with less abundant characters

### 2. Implementation: Building the Happy Tower

1. **Core Strategy:**

   - Track streaks of each character (streak_a, streak_b, streak_c)
   - Track remaining counts (a, b, c)
   - Build string character by character

2. **Decision Making at Each Step:**
   We choose a character if either:
   - It's the most abundant AND hasn't appeared twice in a row
   - OR we're forced to use it because others have appeared twice
3. **Priority System:**

   ```cpp
   if ((a >= b && a >= c && streak_a != 2) ||    // 'a' is most abundant
       (a > 0 && (streak_b == 2 || streak_c == 2))) // forced to use 'a'
   ```

4. **State Updates:**
   After adding a character:
   - Decrement its count
   - Reset other streaks
   - Increment its streak

### Implementation Deep Dive

Let's analyze how the code handles Example 1: a=1, b=1, c=7

```markdown
Initial state: a=1, b=1, c=7
Step 1: 'c' (most abundant) → "c" (c=6)
Step 2: 'c' (most abundant) → "cc" (c=5)
Step 3: 'a' (forced) → "cca" (a=0)
Step 4: 'c' (most abundant) → "ccac" (c=4)
Step 5: 'c' (most abundant) → "ccacc" (c=3)
Step 6: 'b' (forced) → "ccaccb" (b=0)
Step 7: 'c' (only choice) → "ccaccbc"
Step 8: 'c' (only choice) → "ccaccbcc"
```

### 3. Edge Cases and Gotchas

1. **All Zeros:**

   - When any count is 0, we need to handle it gracefully
   - The code naturally handles this by checking remaining counts

2. **Single Character Dominance:**
   - When one character count is much larger than others
   - Example: a=7, b=1, c=0 → "aabaa"
   - Solution ensures optimal distribution of separator characters

> **Footnote:** The beauty of this problem lies in its connection to the concept of "greedy stays ahead." This principle, first formalized by David Johnson in 1974, shows that sometimes being greedily optimal at each step leads to a globally optimal solution. The Happy String problem is a perfect example where local optimization (using the most abundant valid character) naturally leads to the best possible outcome.
