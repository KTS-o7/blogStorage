+++
title = 'Problem Pass the Pillow'
date = 2024-07-06T16:45:27+05:30
draft = false
series = 'leetcode'
tags =['math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2582](https://leetcode.com/problems/pass-the-pillow/description/)

## Question

There are `n` people standing in a line labeled from `1` to `n`. The first person in the line is holding a pillow initially. Every second, the person holding the pillow passes it to the next person standing in the line. Once the pillow reaches the end of the line, the direction changes, and people continue passing the pillow in the opposite direction.

- For example, once the pillow reaches the `nth` person they pass it to the `n - 1th` person, then to the `n - 2th` person and so on.

Given the two positive integers `n` and `time`, return the index of the person holding the pillow after `time` seconds.

## Example 1

```
Input: n = 4, time = 5
Output: 2
Explanation: People pass the pillow in the following way: 1 -> 2 -> 3 -> 4 -> 3 -> 2.
After five seconds, the 2nd person is holding the pillow.
```

## Example 2

```
Input: n = 3, time = 2
Output: 3
Explanation: People pass the pillow in the following way: 1 -> 2 -> 3.
After two seconds, the 3rd person is holding the pillow.
```

## Constraints

```markdown
- 2 <= n <= 1000
- 1 <= time <= 1000
```

## Solution

```cpp
class Solution {
public:
    int passThePillow(int n, int time) {
       int direc = time/(n-1);
       int pos = time%(n-1);
        if(direc%2==0)
            return pos+1;
        else
            return n-pos;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(1)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- Since the pillow is passed in a cyclic manner, we can calculate the direction and position of the person holding the pillow after time seconds.
- The pillow changes direction after every n-1 seconds.
- This can be proved by taking an example of n=4 and time=5.
- at 0th second, the pillow is at 1st position.
- at 1st second, the pillow is at 2nd position.
- at 2nd second, the pillow is at 3rd position.
- at 3rd second, the pillow is at 4th position.
- at 4th second, the pillow is at 3rd position.
- at 5th second, the pillow is at 2nd position.
- Hence, the pillow changes direction after every n-1 seconds.
- If we calculate the number of times the pillow changes direction, we can calculate the position of the person holding the pillow after time seconds.
- Number of times the pillow changes direction = time/(n-1).
- The relative position of the person holding the pillow after time seconds = time%(n-1).
- If the direction is even, the position of the person holding the pillow = pos+1. (this means the pillow was moving from left to right)
- If the direction is odd, the position of the person holding the pillow = n-pos. (this means the pillow was moving from right to left)
```

### 2. Implementation

```markdown
- declare a variable `direc` = time/(n-1).
- This will give the number of times the pillow changes direction.
- declare a variable `pos` = time%(n-1).
- This will give the relative position of the person holding the pillow after time seconds.
- If the direction is even, return pos+1.
- If the direction is odd, return n-pos.
```
