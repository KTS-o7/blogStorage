+++
title = 'Problem K Th Smallest Prime Fraction'
date = 2024-05-10T19:04:08+05:30
draft = false
series = 'leetcode'
tags =['priority-queue','binary-search','array','math','heap','cpp']
toc = false
math = true
+++

# Problem Statement

**Link** - [Problem 786](https://leetcode.com/problems/k-th-smallest-prime-fraction/)

## Question

You are given a sorted integer array `arr` containing `1` and prime numbers, where all the integers of `arr` are **unique**. You are also given an integer `k`.

For every `i` and `j` where `0 <= i < j < arr.length`, we consider the fraction `arr[i] / arr[j]`.

Return the kth smallest fraction considered. Return your answer as an array of integers of size `2`, where `answer[0] == arr[i]` and `answer[1] == arr[j]`.

## Example 1

```text
Input: arr = [1,2,3,5], k = 3
Output: [2,5]
Explanation: The fractions to be considered in sorted order are:
1/5, 1/3, 2/5, 1/2, 3/5, and 2/3.
The third fraction is 2/5.
```

## Example 2

```text
Input: arr = [1,7], k = 1
Output: [1,7]
```

## Constraints

- `2` <= arr.length <= `1000`
- `1` <= arr[i] <= `3 * 10^4`
- arr[0] == `1`
- `arr[i] is a prime number for i > 0`.
- All the numbers of `arr` are **unique** and **sorted in strictly increasing order**.
- `1 <= k <= arr.length * (arr.length - 1) / 2`

## Solution

```cpp
class Solution {
public:
    vector<int> kthSmallestPrimeFraction(vector<int>& arr, int k) {

        std::ios::sync_with_stdio(false);
        cin.tie(0);
        cout.tie(0);

        auto comparePair = [](const pair<int,int>& a,const pair<int,int>& b)
        {
            return a.first*b.second > b.first*a.second;
        };

        priority_queue<pair<int,int>,vector<pair<int,int>>, decltype(comparePair)>minheap;

        for(int i = 0;i<arr.size();i++)
        {
            for(int j=i+1;j<arr.size();j++)
                {
                    minheap.push(make_pair(arr[i],arr[j]));
                }
        }

        vector<int>answer(2,0);
        while(k>1 && !minheap.empty())
        {
            k--;
            minheap.pop();
        }
        answer[0] = minheap.top().first;
        answer[1] = minheap.top().second;
        return answer;
    }
};
```

## Complexity Analysis

- `Time Complexity` - O(n^2logn)
- `Space Complexity` - O(n^2)
- Time complexity of adding a node to the heap is `O(logn)` and we are adding `n*(n-1)/2` nodes to the heap. So, the time complexity is O(n^2logn).
- Space complexity is O(n^2) because we are storing `n*(n-1)/2` pairs in the heap.

## Explanation

### 1. Intuition

- Since we need to find the kth smallest fraction, we can use a min heap to store the fractions.
- For that we need to have all the possible fractions in the heap.
- We can generate all the possible fractions by iterating over the array and adding the fractions to the heap.

### 2. Implementation

- We need to have a min heap whose contents are pairs of integers.
- Each pair {a,b} in the heap represents the fraction a/b.
- We need to define a custom comparator for the heap, because by default the heap will be a max heap.
- The function `comparePair` checks if the fraction a/b is smaller than the fraction c/d.
- {{< math.inline >}}
<p>
 A fraction \(\dfrac{a}{b}\ > \dfrac{c}{d}\ =  a \cdot d > b \cdot c\)
</p>
{{</ math.inline >}}

- The comparator returns `false` if a\*d > b\*c, which means that the fraction a/b is smaller than c/d.
- Since by default the heap is a max heap, we need to make sure that the comparator returns false if a\*d > b\*c.
- Then the node which has {a,b} will move up in the heap than the node which has {c,d}.
- We will iterate over the array and generate all the possible fractions.
- First loop will set the numerator and the second loop will set the denominator.
- We will add the fraction to the heap.
- We will pop k-1 elements from the heap.
- The kth element will be the kth smallest fraction.
- We will return the kth smallest fraction as `answer[0]` and `answer[1]`.

## Additional Notes

- This problem can also be solved using binary search.
- We can use binary search to find the fraction that is the kth smallest.

```cpp
class Solution {
 public:
  vector<int> kthSmallestPrimeFraction(vector<int>& arr, int k) {
    const int n = arr.size();
    double l = 0.0;
    double r = 1.0;

    while (l < r) {
      const double m = (l + r) / 2.0;
      int fractionsNoGreaterThanM = 0;
      int p = 0;
      int q = 1;

      // For each index i, find the first index j s.t. arr[i] / arr[j] <= m,
      // so fractionsNoGreaterThanM for index i will be n - j.
      for (int i = 0, j = 1; i < n; ++i) {
        while (j < n && arr[i] > m * arr[j])
          ++j;
        if (j == n)
          break;
        fractionsNoGreaterThanM += n - j;
        if (p * arr[j] < q * arr[i]) {
          p = arr[i];
          q = arr[j];
        }
      }

      if (fractionsNoGreaterThanM == k)
        return {p, q};
      if (fractionsNoGreaterThanM > k)
        r = m;
      else
        l = m;
    }

    throw;
  }
};
```

- The above code uses binary search to find the kth smallest fraction.
- We start with the range [0,1] and find the middle value.
- We iterate over the array and find the number of fractions that are less than or equal to `m`.
- If the number of fractions is equal to `k`, we return the fraction.
- If the number of fractions is greater than `k`, we reduce the range to [l,m].
- If the number of fractions is less than `k`, we increase the range to [m,r].

```text
Time Complexity - O(nlogn)
Space Complexity - O(1)
```

---

> This problem helps us to understand how to use a min heap to solve a problem that requires finding the kth smallest element.
