+++
title = 'Problem Minimum Number of Moves to Seat Everyone'
date = 2024-06-13T21:56:56+05:30
draft = false
series = 'leetcode'
tags =['greedy','sorting','array']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2037](https://leetcode.com/problems/minimum-number-of-moves-to-seat-everyone/description/)

## Question

There are `n` seats and `n` students in a room. You are given an array `seats` of length `n`, where `seats[i]` is the position of the `ith` seat. You are also given the array `students` of length `n`, where `students[j]` is the position of the `jth` student.

You may perform the following move any number of times:

- Increase or decrease the position of the `ith` student by `1` (i.e., moving the `ith` student from position `x` to `x + 1` or `x - 1`)
  Return the **minimum number of moves** required to move each student to a seat such that no two students are in the same seat.

Note that there may be **multiple** seats or students in the **same** position at the beginning.

#### Note

- This question is very confusing, but the question is asking what is the number of moves required to seat all the students in the seats such that no two students are in the same seat.
- Also the to get minimum moves we need to make sure the student is matched to the seat which is closest to him.

## Example 1

```text
Input: seats = [3,1,5], students = [2,7,4]
Output: 4
Explanation: The students are moved as follows:
- The first student is moved from from position 2 to position 1 using 1 move.
- The second student is moved from from position 7 to position 5 using 2 moves.
- The third student is moved from from position 4 to position 3 using 1 move.
In total, 1 + 2 + 1 = 4 moves were used.
```

## Example 2

```text
Input: seats = [4,1,5,9], students = [1,3,2,6]
Output: 7
Explanation: The students are moved as follows:
- The first student is not moved.
- The second student is moved from from position 3 to position 4 using 1 move.
- The third student is moved from from position 2 to position 5 using 3 moves.
- The fourth student is moved from from position 6 to position 9 using 3 moves.
In total, 0 + 1 + 3 + 3 = 7 moves were used.
```

## Example 3

```text
Input: seats = [2,2,6,6], students = [1,3,2,6]
Output: 4
Explanation: Note that there are two seats at position 2 and two seats at position 6.
The students are moved as follows:
- The first student is moved from from position 1 to position 2 using 1 move.
- The second student is moved from from position 3 to position 6 using 3 moves.
- The third student is not moved.
- The fourth student is not moved.
In total, 1 + 3 + 0 + 0 = 4 moves were used.
```

## Constraints

- `n == seats.length == students.length`
- `1 <= n <= 100`
- `1 <= seats[i], students[j] <= 100`

## Solution

```cpp
class Solution {
public:
    int minMovesToSeat(vector<int>& seats, vector<int>& students) {
        sort(students.begin(),students.end());
        sort(seats.begin(),seats.end());

        int answer = 0;
        for(int i =0;i<seats.size();i++)
        {
            answer+=abs(seats[i]-students[i]);
        }
        return answer;
    }

};
```

## Complexity Analysis

- `Time Complexity` : O(NlogN)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- We need to implement a greedy approach to solve this problem.
- We need to sort the students and seats array.
- Then we can match the students to the seats which are closest to them.
```

### 2. Implementation

```markdown
- First we sort the students and seats array.
- Then we iterate over the seats array and calculate the absolute difference between the seat and student.
- We add the difference to the answer.
- Finally we return the answer.
```
