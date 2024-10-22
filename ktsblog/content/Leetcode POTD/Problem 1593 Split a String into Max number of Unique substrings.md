+++
title = 'Problem 1593 Split a String Into Max Number of Unique Substrings'
date = 2024-10-21T21:03:08+05:30
draft = false
series = 'leetcode'
tags =['backtrack','string','substring','set','backtracking']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1593](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings/description/)

## Question

Given a string `s`, return the maximum number of unique substrings that the given string can be split into.

You can split string `s` into any list of non-empty substrings, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are unique.

A **substring** is a contiguous sequence of characters within a string.

## Example 1

```
Input: s = "ababccc"
Output: 5
Explanation: One way to split maximally is ['a', 'b', 'ab', 'c', 'cc'].
Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.
```

## Example 2

```
Input: s = "aba"
Output: 2
Explanation: One way to split maximally is ['a', 'ba'].
```

## Example 3

```
Input: s = "aa"
Output: 1
Explanation: It is impossible to split the string any further.
```

## Constraints

- 1 <= `s.length` <= 16
- `s` contains only lower case English letters.

## Solution

```cpp
class Solution {
public:
    int maxUniqueSplit(string s) {
        unordered_set<string> seen;
        return backtrack(0, s, seen);
    }
private:
    int backtrack(int start, const string& s, unordered_set<string>& seen) {
        if (start == s.size()) {
            return 0;
        }
        int maxSplits = 0;
        for (int end = start + 1; end <= s.size(); ++end) {
            string substring = s.substr(start, end - start);
            if (seen.find(substring) == seen.end()) {
                seen.insert(substring);
                maxSplits = max(maxSplits, 1 + backtrack(end, s, seen));
                seen.erase(substring);
            }
        }
        return maxSplits;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Backtrack | O(2^n)          | O(n)             |
```

## Explanation

### 1. Intuition

- The problem requires finding maximum number of unique substrings that a string can be split into.
- We need to consider every possible way to split the string while ensuring no duplicates.
- The key observations that lead to the solution:
  1. We need to try different ways of splitting at each position
  2. We need to track which substrings we've already used
  3. We need to be able to undo our choices if they don't lead to optimal solution
  4. This is a perfect case for backtracking because:
     - We have multiple choices at each step (where to end current substring)
     - We need to explore all possibilities systematically
     - We need to track state (used substrings)
     - We need to undo choices to try different combinations
- For example, with string "ababccc":
  - We could take "a" first, then have options for rest
  - Or take "ab" first, leaving different options
  - Need to try all valid combinations to find maximum

### 2. Implementation

```markdown
1. Main Function Setup

   - Create an empty unordered_set to store seen substrings
   - Initialize backtracking from index 0
   - Return result from backtrack function

2. Backtracking Function Components:

   - Parameters:
     - start: current position in string
     - s: original string
     - seen: set of used substrings
   - Returns:
     - Maximum number of splits possible from current position

3. Base Case Handling:

   - If start equals string length
     - Return 0 (no more splits possible)

4. Core Logic Loop:

   - For each possible substring length from current position:
     - Generate substring from start to current end position
     - Check if substring is already used
     - If unique:
       - Add to seen set
       - Make recursive call
       - Remove from seen set (backtrack)
     - Track maximum splits found

5. Processing Steps:

   - Initialize maxSplits = 0
   - For each position 'end' from start+1 to s.length():
     - Create substring from start to end
     - If substring not in seen:
       - Add to seen
       - Recursively process remaining string
       - Update maxSplits if better solution found
       - Remove substring from seen
   - Return maxSplits

6. Example Processing for "ababccc":
   Start = 0:
   - Try "a": Valid → Add to seen → Process "babccc"
   - Try "ab": Valid → Add to seen → Process "abccc"
   - Try "aba": Valid → Add to seen → Process "bccc"
     And so on...
```

Let's see how the code processes a simpler example "aba":

1. First call: start = 0

   - Try "a": seen = {"a"} → Process "ba"
   - Try "ab": seen = {"ab"} → Process "a"
   - Try "aba": seen = {"aba"} → Process ""

2. Second level for "a" split:

   - Processing "ba":
     - Try "b": seen = {"a", "b"} → Process "a"
     - Try "ba": seen = {"a", "ba"} → Process ""

3. Best solution found:
   - ["a", "ba"] giving 2 splits

> The backtracking approach systematically explores all possibilities while maintaining the uniqueness constraint and tracking the maximum number of splits possible.
