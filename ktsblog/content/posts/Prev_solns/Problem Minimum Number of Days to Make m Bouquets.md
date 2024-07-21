+++
title = 'Problem 1482 Minimum Number of Days to Make M Bouquets'
date = 2024-06-19T23:03:42+05:30
draft = false
series = 'leetcode'
tags =['binary-search','array','binary-search-on-answer']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1482](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/description/)

## Question

You are given an integer array `bloomDay`, an integer `m` and an integer `k`.

You want to make `m` bouquets. To make a bouquet, you need to use `k` **adjacent flowers** from the garden.

The garden consists of `n` flowers, the `ith` flower will bloom in the `bloomDay[i]` and then can be used in **exactly one bouquet**.

Return the minimum number of days you need to wait to be able to make `m` bouquets from the garden. If it is impossible to make `m` bouquets return `-1`.

#### Note

Problem description hides 2 crucial points

- we need to collect m bouquests on the SAME given day. Can't collect m-x bouqets on some day, then collect more on another, till we reach m.
  This can be understood only from the examples.
- Once flower bloomed but we can't collect m bouquets -> that flower stays in the garden in the 'bloomed' status.
- Solution is inspired from [Problem 278](https://leetcode.com/problems/first-bad-version/description/)

## Example 1

```text
Input: bloomDay = [1,10,3,10,2], m = 3, k = 1
Output: 3
Explanation: Let us see what happened in the first three days. x means flower bloomed and _ means flower did not bloom in the garden.
We need 3 bouquets each should contain 1 flower.
After day 1: [x, _, _, _, _]   // we can only make one bouquet.
After day 2: [x, _, _, _, x]   // we can only make two bouquets.
After day 3: [x, _, x, _, x]   // we can make 3 bouquets. The answer is 3.
```

## Example 2

```text
Input: bloomDay = [1,10,3,10,2], m = 3, k = 2
Output: -1
Explanation: We need 3 bouquets each has 2 flowers, that means we need 6 flowers. We only have 5 flowers so it is impossible to get the needed bouquets and we return -1.
```

## Example 3

```text
Input: bloomDay = [7,7,7,7,12,7,7], m = 2, k = 3
Output: 12
Explanation: We need 2 bouquets each should have 3 flowers.
Here is the garden after the 7 and 12 days:
After day 7: [x, x, x, x, _, x, x]
We can make one bouquet of the first three flowers that bloomed. We cannot make another bouquet from the last three flowers that bloomed because they are not adjacent.
After day 12: [x, x, x, x, x, x, x]
It is obvious that we can make two bouquets in different ways.
```

## Constraints

```markdown
- `bloomDay.length == n`
- `1 <= n <= 10^5`
- `1 <= bloomDay[i] <= 10^9`
- `1 <= m <= 10^6`
- `1 <= k <= n`
```

## Solution

```cpp
class Solution {
public:
    int minDays(vector<int>& bloomDay, int m, int k) {
        int l = 1, r = 1e9;
        int ans = -1;
        while(l <= r) {
            int mid = l + (r - l) / 2;
            int consecutiveLength = 0, boquets = 0;
            for(int i = 0; i < bloomDay.size(); i++)
            {
                if(bloomDay[i] <= mid)
                {
                    consecutiveLength++;
                    if(consecutiveLength >= k)
                    {
                        consecutiveLength = 0;
                        boquets++;
                    }
                }
                else
                {
                    consecutiveLength = 0;
                }
            }

            if(boquets >= m)
            {
                ans = mid;
                r = mid - 1;
            }
            else
            {
                l = mid + 1;
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(nlogn)
- `Space Complexity` : O(1)

## Explanation

### 1. Intuition

```markdown
- If we know how many flowers are blooming on a given day, we can easily calculate the number of bouquets we can make.
- Ask this question: Is `x` days sufficient to make `m` bouquets?
- If yes it means that we can make `m` bouquets in `x+t` days where `t` is any number of days greater than `x`.
- Hence we need to find the minimum number of days to make `m` bouquets.
```

### 2. Implementation

```markdown
- Initialize `l` and `r` as 1 and 1e9 respectively.
- They represent the minimum and maximum number of days required to make `m` bouquets. (according to the constraints)
- Initialize `ans` as -1.
- This will represent the minimum number of days required to make `m` bouquets.
- Run a while loop till `l <= r`.
- Calculate the mid value as `l + (r - l) / 2`.
- Initialize `consecutiveLength` and `boquets` as 0.
- Run a loop for all the flowers in the garden.
- If the flower blooms on the `mid` day, increment the `consecutiveLength`. This represents the number of flowers blooming consecutively.
- If the `consecutiveLength` is greater than or equal to `k`, reset the `consecutiveLength` and increment the `boquets`.
- If the flower does not bloom on the `mid` day, reset the `consecutiveLength`.
- Once the for loop ends, check if the number of `boquets` is greater than or equal to `m`.
- If yes, update the `ans` as `mid` and update `r` as `mid - 1`.
- This shows that we can make `m` bouquets in `mid` days. So we need to check if they can be made in less than `mid` days.
- If no, update `l` as `mid + 1`.
- This shows that we cannot make `m` bouquets in `mid` days. So we need to check if they can be made in more than `mid` days.
- Return the `ans`.
```

> This is a very good problem to understand the power of binary search. In this problem we use binary search to find the point after which the required condition will always be true. We are trying to use the leftmost binary search to find the minimum number of days required to make `m` bouquets. This is a very good example of binary search on answer.
