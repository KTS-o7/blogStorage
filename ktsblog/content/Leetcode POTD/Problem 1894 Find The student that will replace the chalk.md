+++
title = 'Problem 1894 Find the Student That Will Replace the Chalk'
date = 2024-09-02T16:26:58+05:30
draft = false
series = 'leetcode'
tags =['math','arrays']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1894](https://leetcode.com/problems/find-the-student-that-will-replace-the-chalk/description/)

## Question

There are `n` students in a class numbered from `0` to `n - 1`. The teacher will give each student a problem starting with the student number `0`, then the student number `1`, and so on until the teacher reaches the student number `n - 1`. After that, the teacher will restart the process, starting with the student number `0` again.

You are given a `0-indexed` integer array `chalk` and an integer `k`. There are initially `k` pieces of chalk. When the student number `i` is given a problem to solve, they will use `chalk[i]` pieces of chalk to solve that problem. However, if the current number of chalk pieces is strictly less than `chalk[i]`, then the student number `i` will be asked to replace the chalk.

Return the index of the student that will replace the chalk pieces.

## Example 1

```
Input: chalk = [5,1,5], k = 22
Output: 0
Explanation: The students go in turns as follows:
- Student number 0 uses 5 chalk, so k = 17.
- Student number 1 uses 1 chalk, so k = 16.
- Student number 2 uses 5 chalk, so k = 11.
- Student number 0 uses 5 chalk, so k = 6.
- Student number 1 uses 1 chalk, so k = 5.
- Student number 2 uses 5 chalk, so k = 0.
Student number 0 does not have enough chalk, so they will have to replace it.
```

## Example 2

```
Input: chalk = [3,4,1,2], k = 25
Output: 1
Explanation: The students go in turns as follows:
- Student number 0 uses 3 chalk so k = 22.
- Student number 1 uses 4 chalk so k = 18.
- Student number 2 uses 1 chalk so k = 17.
- Student number 3 uses 2 chalk so k = 15.
- Student number 0 uses 3 chalk so k = 12.
- Student number 1 uses 4 chalk so k = 8.
- Student number 2 uses 1 chalk so k = 7.
- Student number 3 uses 2 chalk so k = 5.
- Student number 0 uses 3 chalk so k = 2.
Student number 1 does not have enough chalk, so they will have to replace it.
```

## Constraints

- chalk.length == n
- 1 <= n <= 10<sup>5</sup>
- 1 <= chalk[i] <= 10<sup>5</sup>
- 1 <= k <= 10<sup>9</sup>

## Solution

```cpp
class Solution {
public:
    int chalkReplacer(vector<int>& chalk, int k) {
        long long tot = 0;
        int size = chalk.size();
        for(int it:chalk){
            tot+=it;
        }
        k = k%tot;
        for(int i =0; i<size;i++){
            if(k<chalk[i])
                return i;
            k-=chalk[i];
        }
        return -1;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm       | Time Complexity | Space Complexity |
| --------------- | --------------- | ---------------- |
| Accumulation    | O(n)            | O(1)             |
| Array Traversal | O(n)            | O(1)             |
| Total           | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

- The question asks us to basically calculate when the chalk will run out.
- Once the array is done, it needs to be repeated.
- So we need to keep track of how many rounds will be complete.
- To find the index which will replace the chalk, we need to find the index where the chalk will run out.
- So the replacement index is decided by the reminder of number of chalks left after all the complete rounds.
- So we need total number of chalks used in one round.
- Then find the reminder of the total chalks used in all rounds.
- Then iterate over the array and find the index where the chalk will run out.
- Return the index.
- Remember to keep the total count of chalks in `long long` to avoid overflow. (due to constraints)

### 2. Implementation

```markdown
- Initialize a variable `tot` to store the total number of chalks used in one round.
- Initialize a variable `size` to store the size of the array.
- Iterate over the array and accumulate the total number of chalks used in one round.
- Find the reminder of the total chalks used in all rounds.
- Iterate from `0` to `size`
  - If the reminder is less than the chalk at index `i`, return `i`.
  - Else decrement the reminder by the chalk at index `i`.
- Return `-1`.
```
