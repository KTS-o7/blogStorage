+++
title = 'Problem Crawler Log Folder'
date = 2024-07-10T21:35:27+05:30
draft = false
series = 'leetcode'
tags =['math','string']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1598](https://leetcode.com/problems/crawler-log-folder/description/)

## Question

The Leetcode file system keeps a log each time some user performs a change folder operation.

The operations are described below:

- `"../`" : Move to the parent folder of the current folder. (If you are already in the main folder, remain in the same folder).
- `"./"` : Remain in the same folder.
- `"x/`" : Move to the child folder named x (This folder is guaranteed to always exist).
- You are given a list of strings `logs` where `logs[i]` is the operation performed by the user at the ith step.

The file system starts in the main folder, then the operations in `logs` are performed.

Return the minimum number of operations needed to go back to the main folder after the change folder operations.

## Example 1

![image](https://assets.leetcode.com/uploads/2020/09/09/sample_11_1957.png)

```
Input: logs = ["d1/","d2/","../","d21/","./"]
Output: 2
Explanation: Use this change folder operation "../" 2 times and go back to the main folder.

```

## Example 2

![image](https://assets.leetcode.com/uploads/2020/09/09/sample_22_1957.png)

```
Input: logs = ["d1/","d2/","./","d3/","../","d31/"]
Output: 3
```

## Example 3

```
Input: logs = ["d1/","../","../","../"]
Output: 0
```

## Constraints

```markdown
- `1 <= logs.length <= 10^3`
- `2 <= logs[i].length <= 10`
- logs[i] contains lowercase English letters, digits, '.', and '/'.
- logs[i] follows the format described in the statement.
- Folder names consist of lowercase English letters and digits.
```

## Solution

```cpp
class Solution {
public:
    int minOperations(vector<string>& logs) {
        int count = 0;
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        for(string it:logs)
        {
            if(it == "../")
            {
                count--;
                if(count<0)
                    count = 0;
            }
            else if(it != "./")
                count++;
        }
        return count;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- Since the directory is Linear in the given problem, we can keep track of the count of the operations.
- If the operation is `../` then we decrement the count and if the count is less than 0, we reset it to 0.
- If the operation is not `./` then we increment the count.
- Finally, we return the count.
```

### 2. Implementation

```markdown
- Initialize the `count` to 0.
- Iterate over the `logs` and check if the operation is `../` then decrement the `count` and reset it to 0 if it is less than 0.
- If the operation is not `./` then increment the `count`.
- Finally, return the `count`.
```
