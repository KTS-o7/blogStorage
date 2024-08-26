+++
title = 'Problem 881 Boats to Save People'
date = 2024-05-04T15:00:06+05:30
draft = false
series = 'leetcode'
tags =['two-pointers','greedy','cpp']
toc = false
+++

# Problem Statement

**Link** - [Problem 881](https://leetcode.com/problems/boats-to-save-people/description/)

## Question

You are given an array `people` where `people[i]` is the weight of the `i`th person, and an _infinite_ number of boats where each boat can carry a maximum weight of `limit`. Each boat carries _at most two people_ at the same time, provided the sum of the weight of those people is at most `limit`.

_Return the minimum number of boats to carry every given person_.

## Example 1

```text
Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)
```

## Example 2

```text
Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)
```

## Example 3

```text
Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)
```

## Constraints

- `1 <= people.length <= 5 * 10^4`
- `1 <= people[i] <= limit <= 3 * 10^4`

## Solution

```cpp
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {

        std::ios_base::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        sort(people.begin(),people.end());
        int left = 0,right = people.size()-1;
        int answer = 0;

        while(left<=right)
        {
            if(people[left]+people[right]>limit)
            {
                answer++;
                right--;
            }
            else
            {
                answer++;
                left++;
                right--;
            }
        }
        return answer;
    }
};
```

## Complexity Analysis

- `Time` : O(nlogn)
- `Space` : O(1)

## Explanation

### 1. Intuition

- We can see that the number of boats needed at minimum is `n/2` where `n` is the number of people.
- Maximum number of boats needed is `n` where each person is in a separate boat.
- The idea is to give the heaviest person a boat and then check if the lightest person can be accomodated in the same boat.
- If the lightest person can be accomodated then we can move to the next lightest person.
- If the lightest person cannot be accomodated then we need to give the heaviest person a separate boat.

### 2. Code explained

- Sort the given vector.
- Initialize two pointers `left` and `right` at the start and end of the vector.
- Initialize the `answer` variable to `0`.
- Iterate over the vector until `left` is less than or equal to `right`.
- If the sum of the weights of the people at `left` and `right` is greater than the `limit` then we need to give the person at `right` a separate boat.
- If the sum of the weights of the people at `left` and `right` is less than or equal to the `limit` then we can accomodate both the people in the same boat.
- Increment the `answer` variable accordingly.
- Return the `answer` variable.

---

> **Note** : This is a very simple problem and can be solved using the two pointer approach. The idea is to sort the given vector and then use two pointers to iterate over the vector. The time complexity of this approach is `O(nlogn)` where `n` is the number of people. The space complexity is `O(1)`.
