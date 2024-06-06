+++
title = 'Problem Hand of Straights'
date = 2024-06-06T23:33:06+05:30
draft = false
series = 'leetcode'
tags =[]
toc = false
+++

# Problem Statement

**Link** - [Problem 846](https://leetcode.com/problems/hand-of-straights/description/)

## Question

Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.

Given an integer array `hand` where `hand[i]` is the value written on the `ith` card and an integer `groupSize`, return true if she can rearrange the cards, or false otherwise.

### Note :

- Only groupSize matters not the number of groups.
- We can only form groups of size `groupSize` if total number of cards is divisible by `groupSize`.
- Consecutive cards means cards with consecutive values ie groups like [1,2,3] or [3,4,5] are valid but [1,3,4] is not valid for given `groupSize = 3`

## Example 1

```text
Input: hand = [1,2,3,6,2,3,4,7,8], groupSize = 3
Output: true
Explanation: Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]
```

## Example 2

```text
Input: hand = [1,2,3,4,5], groupSize = 4
Output: false
Explanation: Alice's hand can not be rearranged into groups of 4.
```

## Constraints

```markdown
- `1 <= hand.length <= 10^4`
- `0 <= hand[i] <= 10^9`
- `1 <= groupSize <= hand.length`
```

## Solution

```cpp
class Solution {
public:
    bool isNStraightHand(vector<int>& hand, int groupSize) {

    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    if (hand.size() % groupSize != 0)
        return false;

    map<int, int> freqCount;

    for (const int& card : hand)
        freqCount[card]++;

    for (auto it = freqCount.begin(); it != freqCount.end(); ++it)
    {
        if (it->second > 0)
        {
            int count = it->second;
            for (int i = 0; i < groupSize; ++i)
            {
                if (freqCount[it->first + i] < count)
                    return false;

                freqCount[it->first + i] -= count;
            }
        }
    }
    return true;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(NlogN)
- `Space Complexity` : O(N)

## Explanation

### 1. Intuition

```markdown
- We need to check if we can form groups of size `groupSize` with given cards.
- If not possible return false.
- We need to maintain the count of each card.
- We need this count to check if we can form groups of size `groupSize`.
```

### 2. Implementation

```markdown
- Check if total number of cards is divisible by `groupSize`.
- If not return `false`.
- Create a map `freqCount` to store the count of each card.
- Iterate over the `hand` and increment the count of each card in `freqCount`.
- For every card in `freqCount` check if the count is greater than `0`.
- If yes then assign the count to a variable `count`.
- Iterate over the next `groupSize` cards and check if the count of each card is greater than or equal to `count`.
- This will help us to check if we can form groups of size `groupSize` with current card and next `groupSize-1` cards.
- If not return `false`.
- If yes then decrement the count of each card by `count`.
- Continue this process until we have checked all the cards.
- If we reach here then return `true`.
```

> This problem is also same as [Problem 1296](https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/description/). We need to dovode the array into groups of size `k` such that each group is consecutive.
