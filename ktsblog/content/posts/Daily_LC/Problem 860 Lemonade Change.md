+++
title = 'Problem 860 Lemonade Change'
date = 2024-08-15T18:20:56+05:30
draft = false
series = 'leetcode'
tags =['greedy','array']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 860](https://leetcode.com/problems/lemonade-change/description/)

## Question

At a lemonade stand, each lemonade costs `$5`. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a `$5`, `$10`, or `$20` bill. You must provide the correct change to each customer so that the net transaction is that the customer pays `$5`.

Note that you do not have any change in hand at first.

Given an integer array `bills` where `bills[i]` is the bill the `ith` customer pays, return `true` if you can provide every customer with the correct change, or `false` otherwise.

## Example 1

```
Input: bills = [5,5,5,10,20]
Output: true
Explanation:
From the first 3 customers, we collect three $5 bills in order.
From the fourth customer, we collect a $10 bill and give back a $5.
From the fifth customer, we give a $10 bill and a $5 bill.
Since all customers got correct change, we output true.
```

## Example 2

```
Input: bills = [5,5,10,10,20]
Output: false
Explanation:
From the first two customers in order, we collect two $5 bills.
For the next two customers in order, we collect a $10 bill and give back a $5 bill.
For the last customer, we can not give the change of $15 back because we only have two $10 bills.
Since not every customer received the correct change, the answer is false.
```

## Constraints

```markdown
- `1 <= bills.length <= 10^5`
- `bills[i] is either 5, 10, or 20.`
```

## Solution

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        std::ios::sync_with_stdio(false);
        int five = 0, ten = 0;
        for(int &it:bills){
            if(it == 5){
                five++;
            }
            else if(it == 10){
               if(five>0){
                ten++;
                five--;
               }
               else{
                return false;
               }
            }
            else{
                if(ten>0 && five>0){
                    ten--;
                    five--;
                }
                else if(five>2){
                    five-=3;
                }
                else{
                    return false;
                }
            }
            //cout<<five<<" "<<ten<<endl;
        }
        return true;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Greedy    | O(N)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- We need to keep track of the way the change is given.
- In real life, we always try to give the change by using higher denominations first.
- We can use the same approach here.
- We can keep track of the number of 5's and 10's we have.
- If 20 is given, we can give 10+5 or 5+5+5.
- If 10 is given, we can give 5.
- If 5 is given, we can keep it.
- If we can't give the change, we return false.
- If we can give the change to all customers, we return true.
```

### 2. Implementation

```markdown
- Initialize `five` and `ten` to 0.
- Iterate over the bills.
- If the bill is 5, increment `five`.
- If the bill is 10, check if we have 5's.
  - If we have 5's, increment `ten` and decrement `five`.
  - If we don't have 5's, return false.
- If the bill is 20, check if we have 10's and 5's.
  - If we have 10's and 5's, decrement `ten` and `five`.
  - If we have 5's, decrement `five` by 3.
  - If we don't have 10's and 5's, return false.
- Return true.
```
