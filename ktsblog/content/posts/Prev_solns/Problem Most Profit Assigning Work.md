+++
title = 'Problem 826 Most Profit Assigning Work'
date = 2024-06-18T20:52:44+05:30
draft = false
series = 'leetcode'
tags =['sorting','heap','max-heap','greedy']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 826](https://leetcode.com/problems/most-profit-assigning-work/description/)

## Question

You have `n` jobs and `m` workers. You are given three arrays: `difficulty`, `profit`, and `worker` where:

- `difficulty[i]` and `profit[i]` are the difficulty and the profit of the `i`th job, and
- `worker[j]` is the ability of `j`th worker (i.e., the `j`th worker can only complete a job with difficulty at most `worker[j]`).
  Every worker can be assigned **at most one** job, but one job can be **completed multiple times**.

- For example, if three workers attempt the same job that pays `$1`, then the total profit will be `$3`. If a worker cannot complete any job, their profit is `$0`.
  Return the maximum profit we can achieve after assigning the workers to the jobs.

## Example 1

```text
Input: difficulty = [2,4,6,8,10],
profit = [10,20,30,40,50],
worker = [4,5,6,7]
Output: 100
Explanation: Workers are assigned jobs of difficulty [4,4,6,6] and
 they get a profit of [20,20,30,30] separately.
```

## Example 2

```text
Input: difficulty = [85,47,57],
profit = [24,66,99],
worker = [40,25,25]
Output: 0
```

## Constraints

```markdown
- n == difficulty.length
- n == profit.length
- m == worker.length
- 1 <= n, m <= 10^4
- 1 <= difficulty[i], profit[i], worker[i] <= 10^5
```

## Solution

```cpp
class Solution {
public:
    int maxProfitAssignment(vector<int>& difficulty, vector<int>& profit, vector<int>& worker) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        vector<pair<int,int>>pq(profit.size());

        for (int i = 0; i < profit.size(); i++)
        {
            pq[i]= {profit[i], difficulty[i]} ;
        }

        int wsize = worker.size(), pqsize = pq.size(),i =0, j=0, ans = 0;

        sort(worker.begin(), worker.end(), greater<int>());
        sort(pq.begin(),pq.end(),greater<pair<int,int>>());

        while (i < wsize && j < pqsize)
        {

            while(j<pqsize && pq[j].second>worker[i])
                j++;

            if(i == wsize || j == pqsize)
                break;

            while(i<wsize && j<pqsize && pq[j].second <= worker[i])
            {
                // cout<<j<<ans<<" "<<pq[j].first<<endl;
                ans+=pq[j].first;
                i++;
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(nlogn)
- `Space Complexity` : O(n)

## Explanation

### 1. Intuition

```markdown
- We can get maximum profit if we keep assigning the maximum profit job repeatedly.
- But we need to ensure that the worker can complete the job.
- So we can create pairs of profit and difficulty and sort them in descending order.
- We can sort the workers in descending order.
- Now we find the first job that the worker can complete and assign it to him.
- We try to assign the same job to all the workers who can complete the job; this way, we can get the maximum profit.
- If a worker cannot complete any job, we move to the next possible job.
```

### 2. Implementation

```markdown
- Initialize a vector of pairs `pq` of size `profit.size()`.
- Fill the vector with pairs of profit and difficulty.
- Initialize `wsize = worker.size()`, `pqsize = pq.size()`, `i = 0`, `j = 0`, and `ans = 0`.
- Sort the workers and the pairs in descending order.
- Run a while loop till `i < wsize` and `j < pqsize`.
- Run a while loop till `j < pqsize` and `pq[j].second > worker[i]`.
- Keep incrementing `j` till we find the first job that the worker can complete.
- This loop will find the first possible job that the worker can complete.
- If `i == wsize` or `j == pqsize`, break.
- This loop will break if we reach the end of the workers or the pairs of jobs.
- Run a while loop till `i < wsize` and `j < pqsize` and `pq[j].second <= worker[i]`.
- This loop represents the assignment of jobs to workers.
- Same job will be assigned to all the workers who can complete the job.
- Increment `i` and add the profit to `ans`.
- Return `ans`.
```

> **Note** - The code can be optimized by using a priority queue instead of sorting the pairs. Also this problem can be solved using a greedy approach as shown.
