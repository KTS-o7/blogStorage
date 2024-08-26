+++
title = 'Problem 506 Relative Ranks'
date = 2024-05-08T14:24:30+05:30
draft = false
series = 'leetcode'
tags =['priority-queue','cpp','vector','strings','heap','custom-comparator']
toc = false
+++

# Problem Statement

**Link** - [Problem 506](https://leetcode.com/problems/relative-ranks/description/)

## Question

You are given an integer array `score` of size `n`, where `score[i]` is the score of the `ith` athlete in a competition. All the scores are guaranteed to be **unique**.

The athletes are placed based on their scores, where the `1st` place athlete has the highest score, the `2nd` place athlete has the 2nd highest score, and so on. The placement of each athlete determines their rank:

- The `1st` place athlete's rank is `"Gold Medal"`.
- The `2nd` place athlete's rank is `"Silver Medal"`.
- The `3rd` place athlete's rank is `"Bronze Medal"`.
- For the `4th` place to the `nth` place athlete, their rank is their placement number (i.e., the `xth` place athlete's rank is `"x"`).

Return an array `answer` of size `n` where `answer[i]` is the rank of the `ith` athlete.

## Example 1

```text
Input: score = [5,4,3,2,1]
Output: ["Gold Medal","Silver Medal","Bronze Medal","4","5"]
Explanation: The placements are [1st, 2nd, 3rd, 4th, 5th].
```

## Example 2

```text
Input: score = [10,3,8,9,4]
Output: ["Gold Medal","5","Bronze Medal","Silver Medal","4"]
Explanation: The placements are [1st, 5th, 3rd, 2nd, 4th].
```

## Constraints

- `n == score.length`
- `1` <= n <= `10^4`
- `0` <= score[i] <= `10^6`
- All the values in score are _unique_.

## Solution

```cpp
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& score) {
        auto comparePair = [](const pair<int,int>& a,const pair<int,int>& b)
        {
            return a.first<b.first;
        };
        priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(comparePair)> maxHeap;

        for(int i = 0;i<score.size();i++)
            maxHeap.push(make_pair(score[i],i));

        vector<string>answer(score.size());
        pair<int,int> holder;
        int place = 0;
        vector<string>places = {"Gold Medal","Silver Medal","Bronze Medal"};
        while(!maxHeap.empty() && place <3)
        {
            holder = maxHeap.top();
            maxHeap.pop();
            answer[holder.second] = places[place];
            place++;
        }
        while(!maxHeap.empty())
        {

            holder = maxHeap.top();
            maxHeap.pop();
            answer[holder.second] = to_string(++place);
        }
        return answer;


    }
};
```

## Complexity Analysis

- `Time`: O(nlogn)
- `Space`: O(n)

## Explanation

### 1. Intuition

- Since the scores need to be relatively ordered in decreasing order, along with the index which they occured.
- We can use a priority queue to store the scores and their index.
- We would need to have a custom comparator to compare the scores.
- This needs to be a max heap, so that we can get the highest score first.

### 2. Implementation

- We can use a priority queue to store the scores and their index.
- We can use a custom comparator to compare the scores.
- The custom comparator `comparePair` compares the pair<score,index_which_it_occured>.
- The lambda function `comparePair` does `a.first<b.first` because internally the priority queue is a max heap, hence the highest score would be at the top.
- The custom comparator ensures that the the pair with highest `score` is at the top of the heap.
- The keyword `decltype` is used to get the type of the lambda function.
- Priority queue needs to have a callable object as a comparator, hence we pass the lambda function `comparePair`.
- We will first push the scores and their index into the priority queue.
- We will have a vector of strings `answer` to store the ranks of the athletes.
- We will have a variable `place` to keep track of the rank.
- We will have a vector of strings `places` to store the `medals` of the athletes.
- We will iterate the priority queue until the `place` is less than `3`.
- We will pop the top element from the priority queue.
- We will assign the rank to the athlete based on the `place`.
- `1st` place athlete's rank is `"Gold Medal"`.
- `2nd` place athlete's rank is `"Silver Medal"`.
- `3rd` place athlete's rank is `"Bronze Medal"`.
- We will increment the `place` after assigning the rank each time.
- Then check if there are any more athletes left.
- If there are any athletes left, assign the rank based on the `place`.
- The ranks is a string hence use the `to_string` function to convert the integer to string.
- Return the `answer` vector.

---

### Alternate Solution

- A solution with a map can be used to store the scores and their index.

```cpp
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& score) {
        int n = score.size();
        map<int, int> mp1;
        vector<string> result(n);

        for (int i = 0; i < n; i++) {
            mp1[score[i]] = i;
        }
        sort(score.begin(), score.end(), greater<int>());
        for (int i = 0; i < n; i++) {
            if (i == 0) {
                result[mp1[score[i]]] = "Gold Medal";
            } else if (i == 1) {
                result[mp1[score[i]]] = "Silver Medal";
            } else if (i == 2) {
                result[mp1[score[i]]] = "Bronze Medal";
            } else {
                result[mp1[score[i]]] = to_string(i + 1);
            }
        }

        return result;
    }
};
```

```text
Time Complexity: O(nlogn)
Space Complexity: O(n)
```

- A solution with built in comparator for priority queue can be used.

```cpp
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& score) {

        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        priority_queue<pair<int,int>> maxHeap;

        for(int i = 0;i<score.size();i++)
            maxHeap.push(make_pair(score[i],i));

        vector<string>answer(score.size());
        pair<int,int> holder;
        int place = 0;
        vector<string>places = {"Gold Medal","Silver Medal","Bronze Medal"};
        while(!maxHeap.empty() && place <3)
        {
            holder = maxHeap.top();
            maxHeap.pop();
            answer[holder.second] = places[place];
            place++;
        }
        while(!maxHeap.empty())
        {

            holder = maxHeap.top();
            maxHeap.pop();
            answer[holder.second] = to_string(++place);
        }
        return answer;


    }
};
```

---

> This problem demonstrtaes the use of priority queue and custom comparators to solve the problem. The alternate solutions are also provided for the same problem.
