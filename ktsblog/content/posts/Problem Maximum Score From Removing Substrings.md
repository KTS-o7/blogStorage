+++
title = 'Problem Maximum Score From Removing Substrings'
date = 2024-07-12T19:10:01+05:30
draft = false
series = 'leetcode'
tags =['stack','string']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1717](https://leetcode.com/problems/maximum-score-from-removing-substrings/description/)

## Question

You are given a string `s` and two integers `x` and `y`. You can perform two types of operations any number of times.

- Remove substring `"ab"` and gain `x` points.
  - For example, when removing `"ab"` from `"cabxbae"` it becomes `"cxbae"`.
- Remove substring `"ba"` and gain y points.
  - For example, when removing `"ba"` from `"cabxbae"` it becomes `"cabxe"`.

Return the maximum points you can gain after applying the above operations on `s`.

## Example 1

```
Input: s = "cdbcbbaaabab", x = 4, y = 5
Output: 19
- Remove "ba" at index 5&6, s = "cdbcbaabab", score = 5.
- Remove "ba" at index 4&5  s = "cdbcabab", score = 10.
- Remove "ba" at index 5&6, s = "cdbcab", score = 15.
- Remove "ab" at index 4&5, s = "cdcb", score = 19.
Total score = 5 + 4 + 5 + 5 = 19.
```

## Example 2

```
Input: s = "aabbaaxybbaabb", x = 5, y = 4
Output: 20
Explanation:
- Remove the "ab" at index 1&2, s = "abaaxybbaabb", score = 5.
- Remove the "ab" at index 0&1, s = "aaxybbaabb", score = 10.
- Remove the "ab" at index 7&8, s = "aaxybbab", score = 15.
- Remove the "ab" at index 6&7, s = "aaxybb", score = 20.
Total score = 5 + 5 + 5 + 5 = 20.
```

## Constraints

```markdown
- `1 <= s.length <= 10^5`
- `1 <= x, y <= 10^4`
- `s` consists of lowercase English letters.
```

#### Note

By seeing the constraints we can say that the solution should be of linear time complexity. hence DP is not a good choice. We can solve this problem using stack.
We will be using a greedy algorithm to solve this problem.

`Why greedy?`
`We can show the following` :

```
Only "aba" and "bab" substrings can potentially lead to a different final score.
Why only those? Because it is possible to remove either "ab" or "ba" in only in those cases.

Now to the proof part:
Assume we have string S. Consider any substring "aba" (or "bab") in it:
prefix of S | aba | suffix of S

notice that removing either ab or ba leads to the same string :
prefix of S | a | suffix of S (or prefix of S | b | suffix of S)

This shows that we must always go for a replacement with the higher value.
```

## Solution

```cpp
class Solution {
public:
    int maximumGain(string s, int x, int y) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        int res = 0;
        string top, bot;
        int top_score, bot_score;

        if (y > x) {
            top = "ba";
            top_score = y;
            bot = "ab";
            bot_score = x;
        } else {
            top = "ab";
            top_score = x;
            bot = "ba";
            bot_score = y;
        }

        stack<char>st;
        for(char ch:s){
            if(ch == top[1] && !st.empty() && st.top() == top[0]){
                res+=top_score;
                st.pop();
            }
            else{
                st.push(ch);
            }
        }
        stack<char>newst;
        char ch;
        while(!st.empty()){
            ch = st.top();
            st.pop();
            if(ch == bot[0] && !newst.empty() && newst.top()==bot[1]){
                res+=bot_score;
                newst.pop();
            }
            else{
                newst.push(ch);
            }
        }

        return res;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N) Because we are iterating over the string once.
- `Space Complexity` : O(N)

## Explanation

### 1. Intuition

```markdown
- First we need to check which substring has higher score.
- Set the `top` string to the substring with higher score.
- Set the `bot` string to the substring with lower score.
- Iterate over the string and check if the top substring can be formed.
- If yes, then add the score to the result.
- Else push the character to the stack.
- Iterate over the stack and check if the bot substring can be formed.
- If yes, then add the score to the result.
- Else push the character to the new stack.
- Return the result.
```

### 2. Implementation

```markdown
- Initialize the result `res` to 0.
- Initialize the `top` and `bot` substrings and their respective scores.
- Check which substring has higher score and set the `top` and `bot` substrings accordingly.
- Initialize a stack `st` and iterate over the string `s`.
- Check if the character is the second character of the `top` substring and the top of the stack is the first character of the `top` substring.
- This is because stack stores the string in reverse order.
- If the condition is true, then add the score to the result and pop the top of the stack.
- Else push the character to the stack.

- Initialize a new stack `newst` and a character `ch`.
- Iterate over the stack `st` and check if the character is the first character of the `bot` substring and the top of the new stack is the second character of the `bot` substring.
- If the condition is true, then add the score to the result and pop the top of the new stack.
- Else push the character to the new stack.
- Return the result.
```
