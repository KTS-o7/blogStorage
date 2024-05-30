+++
title = 'Problem Palindrome Partitioning'
date = 2024-05-22T23:39:58+05:30
draft = false
series = 'leetcode'
tags =['backtracking','recursion','cpp']
toc = false
+++

# Problem Statement

**Link** - [Problem 131](https://leetcode.com/problems/palindrome-partitioning/description/)

## Question

Given a string `s`, partition `s` such that every substring of the partition is a palindrome.
Return all possible palindrome partitioning of s.

### Note to self

- First we need to split the string into all possible substrings.
- Then we need to check if each substring is a palindrome or not.
- If that split is palindromic then we need to add it to the result.

## Example 1

```text
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

## Example 2

```text
Input: s = "a"
Output: [["a"]]
```

## Additional Examples

```text
s = "aaab"
output = [["a","a","a","b"],["a","aa","b"],["aa","a","b"],["aaa","b"]]

s = "abcaa"
output = [["a","b","c","a","a"],["a","b","c","aa"]]

s = "abbab"
output = [["a","b","b","a","b"],["a","b","bab"],["a","bb","a","b"],["abba","b"]]

s = "abaca"
output = [["a","b","a","c","a"],["a","b","aca"],["aba","c","a"]]

s = "aaa"
output = [["a","a","a"],["a","aa"],["aa","a"],["aaa"]]
```

## Constraints

- `1 <= s.length <= 16`
- `s` contains only lowercase English letters.

## Solution

```cpp
class Solution {
public:
    vector<vector<string>> partition(string s) {
        std::ios::sync_with_stdio(false);
        cin.tie(0);
        cout.tie(0);
        vector<vector<string>> result;
        vector<string> path;
        backtrack(s, 0, path, result);
        return result;
    }

private:
    void backtrack(const string& s, int start, vector<string>& path, vector<vector<string>>& result) {

        if (start == s.length())
        {
            result.push_back(path);
            return;
        }

        for (int end = start + 1; end <= s.length(); ++end)
        {
            if (isPalindrome(s, start, end - 1))
            {
                path.push_back(s.substr(start, end - start));
                backtrack(s, end, path, result);
                path.pop_back();
            }
        }
    }

    bool isPalindrome(const string& s, int left, int right) {

        while (left < right)
        {
            if (s[left++] != s[right--])
                return false;
        }
        return true;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N \* 2^N) : 2^N subsets and each subset has N elements
- `Space Complexity` : O(N) : Recursive stack space

## Explanation

### 1. Intuition

- The intuition behind the solution is to use backtracking to generate all possible partitions of the input string `s`.
- For each **_position in the string_**, the algorithm considers extending the current partition with substrings starting from that position.
- Before extending, it checks if the substring is a palindrome using the `isPalindrome` helper function. If the substring is a palindrome, it adds the substring to the current partition (path) and recursively explores further partitions starting from the next character.
- Once a full partition is found (i.e., the end of the string is reached), it adds the partition to the `result` set.

### 2. Implementation

- `Backtracking Function:` The backtrack function is the core of the solution. It takes the input string s, the current start position, the current partition path, and the result set result.
- It iterates through the string, considering each position as a potential start of a new substring. For each position, it checks if the substring ending at that position is a palindrome.
- If so, it extends the current partition with this substring and recursively calls itself to explore further partitions. When the end of the string is reached, it means a valid partition has been found, which is then added to the result set.
- `Is Palindrome Helper Function:` The isPalindrome function checks if a given substring is a palindrome.
- It compares characters from both ends of the substring moving towards the center.
- If any pair of characters does not match, it returns false; otherwise, it continues comparing until the middle of the substring is reached, indicating that the entire substring is a palindrome.

### 3. Dry Run

- Let's dry run the solution on the example input s = "aab" to understand how the backtracking algorithm generates all possible palindrome partitions.

```text
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

```markdown
- First, the backtrack function is called with the input string "aab", start position 0, an empty path, and an empty result set.
- At position 0, the substring "a" is a palindrome, so it is added to the path, and the function is recursively called with the updated path and the next position.
- At position 1, the substring "a" is a palindrome, so it is added to the path, and the function is recursively called with the updated path and the next position.
- At position 2, the substring "b" is a palindrome, so it is added to the path, and the function reaches the end of the string, adding the current partition ["a","a","b"] to the result set.
- Backtracking occurs, and the last character "b" is removed from the path.
- At position 1, the substring "aa" is a palindrome, so it is added to the path, and the function is recursively called with the updated path and the next position.
- At position 2, the substring "b" is a palindrome, so it is added to the path, and the function reaches the end of the string, adding the current partition ["aa","b"] to the result set.
- Backtracking occurs, and the last character "b" is removed from the path.
- Backtracking occurs again, and the last character "a" is removed from the path.
- The function returns the final result set containing all possible palindrome partitions of the input string "aab".
```

> This solution uses backtracking to generate all possible palindrome partitions of the input string. By exploring all possible partitions, it ensures that no valid partition is missed, resulting in a complete set of palindrome partitions.
