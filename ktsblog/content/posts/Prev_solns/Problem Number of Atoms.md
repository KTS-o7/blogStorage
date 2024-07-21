+++
title = 'Problem 726 Number of Atoms'
date = 2024-07-14T22:24:15+05:30
draft = false
series = 'leetcode'
tags =['string','stack','hashmap']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 726](https://leetcode.com/problems/number-of-atoms/description/)

## Question

Given a string `formula` representing a chemical formula, return the count of each atom.

The atomic element always starts with an uppercase character, then zero or more lowercase letters, representing the name.

One or more digits representing that element's count may follow if the count is greater than `1`. If the count is `1`, no digits will follow.

- For example, `"H2O"` and `"H2O2"` are possible, but `"H1O2"` is impossible.

Two formulas are concatenated together to produce another formula.

- For example, `"H2O2He3Mg4"` is also a formula.

A formula placed in parentheses, and a count (optionally added) is also a formula.

- For example, `"(H2O2)"` and `"(H2O2)3"` are formulas.

Return the count of all elements as a string in the following form: the first name (in sorted order), followed by its count (if that count is more than `1`), followed by the second name (in sorted order), followed by its count (if that count is more than `1`), and so on.

The test cases are generated so that all the values in the output fit in a 32-bit integer.

## Example 1

```
Input: formula = "H2O"
Output: "H2O"
Explanation: The count of elements are {'H': 2, 'O': 1}.
```

## Example 2

```
Input: formula = "Mg(OH)2"
Output: "H2MgO2"
Explanation: The count of elements are {'H': 2, 'Mg': 1, 'O': 2}.
```

## Example 3

```
Input: formula = "K4(ON(SO3)2)2"
Output: "K4N2O14S4"
Explanation: The count of elements are {'K': 4, 'N': 2, 'O': 14, 'S': 4}.
```

## Constraints

```markdown
- `1 <= formula.length <= 1000`
- `formula` consists of English letters, digits, `'('`, and `')'`.
- `formula` is always valid.
```

## Solution

```cpp
class Solution {
public:
    string countOfAtoms(string formula) {
        stack<unordered_map<string, int>> stack;
        stack.push(unordered_map<string, int>());

        int index = 0;

        while (index < formula.length()) {

            if (formula[index] == '(') {
                stack.push(unordered_map<string, int>());
                index++;
            }
            else if (formula[index] == ')') {
                unordered_map<string, int> currMap = stack.top();
                stack.pop();
                index++;
                string multiplier;
                while (index < formula.length() && isdigit(formula[index])) {
                    multiplier += formula[index];
                    index++;
                }
                if (!multiplier.empty()) {
                    int mult = stoi(multiplier);
                    for (auto& [atom, count] : currMap) {
                        currMap[atom] = count * mult;
                    }
                }

                for (auto& [atom, count] : currMap) {
                    stack.top()[atom] += count;
                }
            }
            else {
                string currAtom;
                currAtom += formula[index];
                index++;
                while (index < formula.length() && islower(formula[index])) {
                    currAtom += formula[index];
                    index++;
                }

                string currCount;
                while (index < formula.length() && isdigit(formula[index])) {
                    currCount += formula[index];
                    index++;
                }

                int count = currCount.empty() ? 1 : stoi(currCount);
                stack.top()[currAtom] += count;
            }
        }


        map<string, int> finalMap(stack.top().begin(), stack.top().end());

        string ans;
        for (auto& [atom, count] : finalMap) {
            ans += atom;
            if (count > 1) {
                ans += to_string(count);
            }
        }

        return ans;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` : O(N)

## Explanation

### 1. Intuition

```markdown
- Given a string `formula` representing a chemical formula, we need to return the count of each atom.
- We need to recursively parse the formula to get proper count of each atom.
- We can use hash maps to count the atoms inbetween a given set of parenthesis.
- We can use stack to keep track of the hash maps which has been used to so far.
- First have a hashmaps to keep track of the count of each atom.
- Push it into the stack.
- Traverse the formula string.
- If we encounter a `(`, then push an empty hashmap into the stack.
- If we encounter a `)`, then pop the top hashmap from the stack.
- If there is a number after the `)`, then multiply the count of each atom in the popped hashmap with the number.
- Add the count of each atom in the popped hashmap to the top hashmap in the stack.
- If we encounter a character, then add the count of the atom to the top hashmap in the stack.
- Finally, we will have the count of each atom in the top hashmap in the stack.
- Convert the hashmap to a map and then iterate over the map to get the final answer as a string
```

### 2. Implementation

```markdown
- Initialize a stack of hashmaps `stack` and push an empty hashmap into the stack.
- Initialize an integer `index` to 0.
- Traverse the formula string until the end.
- If the character at the `index` is `(`, then push an empty hashmap into the `stack` and increment the `index`.
- If the character at the `index` is `)`, then pop the top hashmap from the `stack` and increment the `index`.
- Initialize a string `multiplier` to empty.
- Traverse the formula string until the end and the character at the `index` is a digit.
- Add the digit to the `multiplier` and increment the `index`.
- If the `multiplier` is not empty, then convert the `multiplier` to an integer `mult`.
- Multiply the count of each atom in the popped hashmap with the `mult`.
- Add the count of each atom in the popped hashmap to the top hashmap in the stack.
- If the character at the `index` is a character, then initialize a string `currAtom` with the character at the `index` and increment the `index`.
- Traverse the formula string until the end and the character at the `index` is a lowercase character.
- Add the lowercase character to the `currAtom` and increment the `index`.
- Initialize a string `currCount` to empty.
- Traverse the formula string until the end and the character at the `index` is a digit.
- Add the digit to the `currCount` and increment the `index`.
- If the `currCount` is empty, then convert the `currCount` to an integer `count`.
- Add the `count` to the `currAtom` in the top hashmap in the stack.
- Convert the top hashmap in the stack to a map `finalMap`.
- Initialize a string `ans` to empty.
- Iterate over the `finalMap` and add the atom to the `ans`.
- If the count of the atom is greater than 1, then add the count to the `ans`.
- Return the `ans`.
```

> This is indeed a tricky question but has a lot of edge cases.
