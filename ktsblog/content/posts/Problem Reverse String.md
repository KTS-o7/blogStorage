+++
title = 'Problem Reverse String'
date = 2024-06-02T16:56:53+05:30
draft = false
series = 'leetcode'
tags =['two-pointer','string']
toc = false
+++

# Problem Statement

**Link** - [Problem 344](https://leetcode.com/problems/reverse-string/description/)

## Question

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array **in-place** with **O(1)** extra memory.

## Example 1

```text
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

## Example 2

```text
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

## Constraints

- `1 <= s.length <= 10^5`
- `s[i]` is a printable ascii character.

## Solution

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int start = 0, end = s.size()-1;
        char ch;
        while(start<end)
        {
            ch = s[start];
            s[start] = s[end];
            s[end] = ch;
            ++start;
            --end;
        }
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` :O(1)

## Explanation

### 1. Intuition

```markdown
- Use two pointer approach to swap the elements.
- We need to write a swapping logic without using any extra space.
```

### 2. Implementation

```markdown
1. Initialize two pointers `start` and `end` to `0` and `s.size()-1` respectively.
2. Swap the elements at `start` and `end` index.
3. Increment `start` and decrement `end` until `start` is less than `end`.
```

## Alternate Solution

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        reverse(s.begin(),s.end());
    }
};
```

### Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` :O(1)

### Explanation

```markdown
- This solution uses the inbuilt `reverse` function to reverse the string.
```

---

> Utilizes the two pointer approach to swap the elements in the array.
