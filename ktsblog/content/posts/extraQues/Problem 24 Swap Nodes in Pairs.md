+++
title = 'Problem 24 Swap Nodes in Pairs'
date = 2024-07-25T20:55:46+05:30
draft = false
series = 'leetcode'
tags =['linked-list','iterative','two-pointer']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 24](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

## Question

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

## Example 1

![image1](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]

```

## Example 2

```
Input: head = []
Output: []
```

## Example 3

```
Input: head = [1]
Output: [1]
```

## Constraints

```markdown
- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`
```

## Solution

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        ListNode* dummy = new ListNode(0,head);
        ListNode* prev = dummy;
        ListNode* curr = head;
        while(curr!=nullptr && curr->next!=nullptr){
            prev->next = curr->next;
            curr->next = prev->next->next;
            prev->next->next = curr;
            prev = curr;
            curr = curr->next;
        }
        return dummy->next;

    }
};
```

## Complexity Analysis

```markdown
| Algorithm   | Time Complexity | Space Complexity |
| ----------- | --------------- | ---------------- |
| Two Pointer | O(N)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- We can add a dummy node at the start of the linked list.
- Then iterate through the Linked list and do the following
  - Set next of prev to next of curr
  - Set next of curr to next of next of previous
  - Set next of next of next of prev to curr
  - move curr to next of curr
- This will set the next of `ith` node to `i+2th` node and next of `i+1th` node to `i+3rd` node
- Then set the next of `i+2th` node to `i+1th` node
- move curr to next of curr
```

### 2. Implementation

```markdown
- Create a dummy node and set its next to head
- Create two pointers prev and curr and set them to dummy and head respectively
- Iterate through the linked list until curr and curr->next are not null
  - Set next of prev to next of curr
  - Set next of curr to next of next of previous
  - Set next of next of next of prev to curr
  - move curr to next of curr
- return dummy->next
```
