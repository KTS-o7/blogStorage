+++
title = 'Problem 82 Remove Duplicates From Sorted List II'
date = 2024-07-28T20:59:29+05:30
draft = false
series = 'leetcode'
tags =['two-pointer','two-pointers','Linked-list']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 82](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/description/)

## Question

Given the `head` of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list sorted as well.

## Example 1

![image1](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)

```
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
```

## Example 2

![image2](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)

```
Input: head = [1,1,1,2,3]
Output: [2,3]
```

## Constraints

```markdown
- `The number of nodes in the list is in the range [0, 300].`
- `-100 <= Node.val <= 100`
- The list is guaranteed to be sorted in ascending order.
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
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr || head->next ==nullptr)
            return head;
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;

        ListNode* prev = dummy;
        ListNode* curr = head;

        while(curr != nullptr && curr->next!=nullptr){
            if(curr->val != curr->next->val){
                prev = curr;
                curr=curr->next;
            }
            else{
                while(curr!=nullptr && prev->next->val == curr->val){
                    curr = curr->next;
                }
                prev->next = curr;
            }
        }
        return dummy->next;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm    | Time Complexity | Space Complexity |
| ------------ | --------------- | ---------------- |
| Two Pointers | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- The logic is follows.
- Add a dummy node behind the head.
- `prev` points to dummy.
- `curr` points to dummy->next or head.
- until the `curr` and `curr->next` is not `nullptr`
  - if value of curr is not equal to the value of next of curr
    - increment prev to curr
    - increment curr to next of curr;
  - else while value of next of prev is equal to value of curr
    - increment curr
  - set prev to curr
- return dummy->next;
```

### 2. Implementation

```markdown
- Initialize a `dummy` node behind the `head`.
- `dummy->next` points to `head`.
- `prev` points to `dummy`.
- `curr` points to `head`.
- until the `curr` and `curr->next` is not `nullptr`
  - if value of curr is not equal to the value of next of curr
    - increment prev to curr
    - increment curr to next of curr;
  - else while value of next of prev is equal to value of curr
    - increment curr
  - set prev to curr
- return dummy->next;
```
