+++
title = 'Problem 1190 Reverse Substrings Between Each Pair of Parentheses'
date = 2024-07-11T22:07:28+05:30
draft = false
series = 'leetcode'
tags =['string','stack','queue']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1190](https://leetcode.com/problems/reverse-substrings-between-each-pair-of-parentheses/description/)

## Question

You are given a string `s` that consists of lower case English letters and brackets.

Reverse the strings in each pair of matching parentheses, starting from the innermost one.

Your result should **not** contain any brackets.

## Example 1

```
Input: s = "(abcd)"
Output: "dcba"
```

## Example 2

```
Input: s = "(u(love)i)"
Output: "iloveu"
Explanation: The substring "love" is reversed first, then the whole string is reversed.
```

## Example 3

```
Input: s = "(ed(et(oc))el)"
Output: "leetcode"
Explanation: First, we reverse the substring "oc", then "etco", and finally, the whole string.
```

## Constraints

```markdown
- `1 <= s.length <= 2000`
- `s` only contains lower case English characters and parentheses.
- It is guaranteed that all parentheses are balanced.
```

## Solution

```cpp
class Solution {
public:
    string reverseParentheses(string s) {

        stack<char> st;
        queue<char> q;
        string ans;

        for(char i : s){
            if(i!=')')
                st.push(i);
            else{
                while(st.top()!='('){
                    q.push(st.top());
                    st.pop();
                }
                st.pop();
                while(!q.empty()){
                    st.push(q.front());
                    q.pop();
                }
            }
        }

        while(!st.empty()){
            ans.push_back(st.top());
            st.pop();
        }

        reverse(ans.begin(),ans.end());

        return ans;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N) Because we are iterating over the string once.
- `Space Complexity` : O(N)

## Explanation

### 1. Intuition

```markdown
- We can use a stack and a queue to solve this problem.
- We can iterate over the string and push the characters into the stack.
- Till we encounter a closing bracket, we keep pushing the characters into the stack.
- Once we encounter a closing bracket, we pop the characters from the stack and push them into the queue till we encounter an opening bracket.
- Once we encounter an opening bracket, we pop the opening bracket from the stack.
- We then push the characters from the queue back into the stack.
- This will effectively reverse the characters between the brackets.
- Once we process the entire string, we can pop the characters from the stack and append them to the answer string.
- Finally, we reverse the answer string and return it.
```

### 2. Implementation

```markdown
- Initialize a stack `st`, a queue `q`, and a string `ans`.
- Iterate over the string `s` and check if the character is not a closing bracket, then push it into the stack.
- If the character is a closing bracket, then pop the characters from the stack and push them into the queue till we encounter an opening bracket.
- Once we encounter an opening bracket, pop the opening bracket from the stack.
- Push the characters from the queue back into the stack.
- Once we process the entire string, pop the characters from the stack and append them to the answer string.
- Reverse the answer string and return it.
```
