+++
title = 'Problem 1395 Count Number of Teams'
date = 2024-07-29T18:30:07+05:30
draft = false
series = 'leetcode'
tags =['dp','dynamic-programming','vectors']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1395](https://leetcode.com/problems/count-number-of-teams/description/)

## Question

There are `n` soldiers standing in a line. Each soldier is assigned a **unique** `rating` value.

You have to form a team of 3 soldiers amongst them under the following rules:

- Choose 3 soldiers with index (`i`, `j`, `k`) with rating (`rating[i]`, `rating[j]`, `rating[k]`).
- A team is valid if: (`rating[i] < rating[j] < rating[k]`) or (`rating[i] > rating[j] > rating[k]`) where (`0 <= i < j < k < n`).

Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

### Note

They want us to count the number of 3-membered sub sequences which satisfiy the rating requirement.

## Example 1

```
Input: rating = [2,5,3,4,1]
Output: 3
Explanation: We can form three teams given the conditions.
(2,3,4), (5,4,1), (5,3,1).
```

## Example 2

```
Input: rating = [2,1,3]
Output: 0
Explanation: We can't form any team given the conditions.
```

## Example 3

```
Input: rating = [1,2,3,4]
Output: 4
```

## Constraints

```markdown
- `n == rating.length`
- `3 <= n <= 1000`
- `1 <= rating[i] <= 10^5`
- All the integers in `rating` are unique.
```

## Solution

```cpp
// Optimal Code
class Solution {
public:
    int numTeams(vector<int>& rating) {
        int count=0,size = rating.size();
        for(int mid = 0; mid<size; mid++){
            int leftSmall = 0;
            int rightBig = 0;
            int leftBig,rightSmall;

            for(int l=mid-1;l>=0;l--){
                if(rating[l]<rating[mid])
                    leftSmall++;
            }

            for(int r =mid+1;r<size;r++){
                if(rating[mid]<rating[r])
                    rightBig++;
            }

            leftBig = mid-leftSmall;
            rightSmall = size-1-mid-rightBig;

            count+= (leftSmall*rightBig) + (leftBig*rightSmall);
        }
        return count;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm           | Time Complexity | Space Complexity |
| ------------------- | --------------- | ---------------- |
| Dynamic programming | O(n^2)          | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- We consider each soldier as a middle memeber of the sub sequence.
- Then count the number of soldiers who have less rating than middle member
  towards the left side.
- Count the number of soldiers who have higher rating than the middle memeber
  towards the right side.
- Total possible subsequences of form (`rating[i]<rating[j]<rating[k]`) will be
  (lowrated left members X high rated right members.)
- Similarly derive the number of high rated members on left side by
  `middle - lower left`
- Derive low rated right members by `size-1-middle - higher left`.
- multiply them to get subsequences of form (`rating[i]>rating[j]>rating[k]`)
- Do this for every member and we will get total count.
```

### 2. Implementation

```markdown
- Initialize the `count` to 0.
- Store the size of the `rating` array at `size`.
- Iterate over the `rating` array.
  - For each member consider it as middle member.
  - Initialize `leftSmall` and `rightBig` to 0.
  - Iterate over the left side of the middle member.
    - If the rating of the member is less than the middle member increment `leftSmall`.
  - Iterate over the right side of the middle member.
    - If the rating of the member is greater than the middle member increment `rightBig`.
  - Calculate the `leftBig` and `rightSmall` members.
  - Increment the `count` by the product of `leftSmall` and `rightBig` and `leftBig` and `rightSmall`.
- Return the `count`.
```

### Unoptimised code

First intuition was to use brute force

```cpp
class Solution {
public:
    int numTeams(vector<int>& rating) {
        int count=0,size = rating.size();
        for(int i=0;i<size-2;i++){
            for(int j=i+1;j<size-1;j++){
                for(int k=j+1;k<size;k++){
                    if(rating[i]<rating[j] && rating[j]<rating[k])
                        count++;
                    if(rating[i]>rating[j] && rating[j]>rating[k])
                        count++;
                }
            }
        }
        return count;
    }
};
```

| Time Complexity | Space Complexity |
| --------------- | ---------------- |
| O(n^3)          | O(1)             |

Then there is a better approach to solve this problem using dynamic programming.

Memoization technique

**Algorithm**

- Initialize
  - `n` as the size of the `rating` array.
  - `teams` to store the number of teams.
  - Two arrays `increasingCache` and `decreasingCache` of size `n*4` to serve as cache.
- Loop over the `rating`. For each index `startIndex`:
  - Call `countIncreasingTeams` and `countDecreasingTeams` with `startIndex`. Add the result to `teams`.
- Return `teams`.

1. Helper method `countIncreasingTeams`:

- Inputs : `rating`,`currentIndex`,`teamSize` and cache `increasingCache`.
- Initialize `n` as the length of the `rating` array.
- If `teamSize` is 3, return 1.
- If `currentIndex` is greater than or equal to `n`, return 0.
- If the value at `increasingCache[currentIndex][teamSize]` is not -1, return the value.
- Initialize a variable `validTeams` to 0.
- Loop from `currentIndex+1` to end of array. For each index `nextIndex`:
  - If `rating[nextIndex] > rating[currentIndex]`, increment `validTeams` by `countIncreasingTeams` with `nextIndex` and `teamSize+1`.
- Store `validTeams` in `increasingCache[currentIndex][teamSize]`.

2. Helper method `countDecreasingTeams`:
   - Similar to `countIncreasingTeams` but with the condition `rating[nextIndex] < rating[currentIndex]`.

```cpp
class Solution {
public:
    int numTeams(vector<int>& rating) {
        int n = rating.size();
        int teams = 0;
        vector<vector<int>> increasingCache(n, vector<int>(4, -1));
        vector<vector<int>> decreasingCache(n, vector<int>(4, -1));

        // Calculate total teams by considering each soldier as a starting point
        for (int startIndex = 0; startIndex < n; startIndex++) {
            teams +=
                countIncreasingTeams(rating, startIndex, 1, increasingCache) +
                countDecreasingTeams(rating, startIndex, 1, decreasingCache);
        }

        return teams;
    }

private:
    int countIncreasingTeams(const vector<int>& rating, int currentIndex,
                             int teamSize,
                             vector<vector<int>>& increasingCache) {
        int n = rating.size();

        // Base case: reached end of array
        if (currentIndex == n) return 0;

        // Base case: found a valid team of size 3
        if (teamSize == 3) return 1;

        // Return cached result if available
        if (increasingCache[currentIndex][teamSize] != -1) {
            return increasingCache[currentIndex][teamSize];
        }

        int validTeams = 0;

        // Recursively count teams with increasing ratings
        for (int nextIndex = currentIndex + 1; nextIndex < n; nextIndex++) {
            if (rating[nextIndex] > rating[currentIndex]) {
                validTeams += countIncreasingTeams(
                    rating, nextIndex, teamSize + 1, increasingCache);
            }
        }

        // Cache and return the result
        return increasingCache[currentIndex][teamSize] = validTeams;
    }

    int countDecreasingTeams(const vector<int>& rating, int currentIndex,
                             int teamSize,
                             vector<vector<int>>& decreasingCache) {
        int n = rating.size();

        // Base case: reached end of array
        if (currentIndex == n) return 0;

        // Base case: found a valid team of size 3
        if (teamSize == 3) return 1;

        // Return cached result if available
        if (decreasingCache[currentIndex][teamSize] != -1) {
            return decreasingCache[currentIndex][teamSize];
        }

        int validTeams = 0;

        // Recursively count teams with decreasing ratings
        for (int nextIndex = currentIndex + 1; nextIndex < n; nextIndex++) {
            if (rating[nextIndex] < rating[currentIndex]) {
                validTeams += countDecreasingTeams(
                    rating, nextIndex, teamSize + 1, decreasingCache);
            }
        }

        // Cache and return the result
        return decreasingCache[currentIndex][teamSize] = validTeams;
    }
};
```

| Time Complexity | Space Complexity |
| --------------- | ---------------- |
| O(n^2)          | O(n) + O(n)      |

Tabulation technique

```cpp
class Solution {
public:
    int numTeams(vector<int>& rating) {
        int n = rating.size();
        int teams = 0;

        // Tables for increasing and decreasing sequences
        vector<vector<int>> increasingTeams(n, vector<int>(4, 0));
        vector<vector<int>> decreasingTeams(n, vector<int>(4, 0));

        // Fill the base cases. (Each soldier is a sequence of length 1)
        for (int i = 0; i < n; i++) {
            increasingTeams[i][1] = 1;
            decreasingTeams[i][1] = 1;
        }

        // Fill the tables
        for (int count = 2; count <= 3; count++) {
            for (int i = 0; i < n; i++) {
                for (int j = i + 1; j < n; j++) {
                    if (rating[j] > rating[i]) {
                        increasingTeams[j][count] +=
                            increasingTeams[i][count - 1];
                    }
                    if (rating[j] < rating[i]) {
                        decreasingTeams[j][count] +=
                            decreasingTeams[i][count - 1];
                    }
                }
            }
        }

        // Sum up the results (All sequences of length 3)
        for (int i = 0; i < n; i++) {
            teams += increasingTeams[i][3] + decreasingTeams[i][3];
        }

        return teams;
    }
};
```

| Time Complexity | Space Complexity |
| --------------- | ---------------- |
| O(n^2)          | O(n) + O(n)      |

> there is a better approach to solve this problem using Fenwick Tree. Can be read at [Link](https://leetcode.com/problems/count-number-of-teams/editorial)
