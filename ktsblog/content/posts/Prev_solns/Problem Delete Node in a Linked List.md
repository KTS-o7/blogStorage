+++
title = 'Problem 237 Delete Node in a Linked List'
date = 2024-05-05T11:54:45+05:30
draft = false
series = 'leetcode'
tags =['linked-list','pointers','cpp']
toc = false
+++

# Problem Statement

**Link** - [Problem 237](https://leetcode.com/problems/delete-node-in-a-linked-list/description/)

## Question

There is a singly-linked list `head` and we want to delete a node `node` in it.

You are given the node to be deleted `node`. You will **not be given access to the first node of head**.

All the values of the linked list are **unique**, and it is guaranteed that the given node `node` is not the last node in the linked list.

Delete the given node. Note that by deleting the node, we mean that :

- The value of the given node should not exist in the linked list.
- The number of nodes in the linked list should decrease by one.
- All the values before `node` should be in the same order.
- All the values after `node` should be in the same order.

## Example 1

```text
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```

- Before deletion

{{<mermaid>}}
graph LR
A((4)) --> B((5))
B --> C((1))
C --> D((9))
{{</mermaid>}}

- After deletion

{{<mermaid>}}
graph LR
A((4)) --> C((1))
C --> D((9))
{{</mermaid>}}

## Example 2

```text
Input: head = [4,5,1,9], node = 1
Output: [4,5,9]
Explanation: You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function.
```

- Before deletion

{{<mermaid>}}
graph LR
A((4)) --> B((5))
B --> C((1))
C --> D((9))
{{</mermaid>}}

- After deletion

{{<mermaid>}}
graph LR
A((4)) --> B((5))
B --> D((9))
{{</mermaid>}}

## Constraints

- The number of the nodes in the given list is in the range `[2, 1000]`.
- `-1000` <= Node.val <= `1000`
- The value of each `node` in the list is **unique**.
- The `node` to be deleted **is in the list** and is **not a tail node**.

## Solution

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        ListNode* toDel = node->next;
        node->val = toDel->val;
        node->next = toDel->next;
        delete toDel;
    }
};
```

## Complexity Analysis

- `Time complexity` : O(1)
- `Space complexity` : O(1)

## Explanation

### 1. Intuition

- We are given the node to be deleted and we can't access the head of the linked list.
- We can't delete the node directly as we don't have access to the previous node.
- The idea is to copy the value of the next node to the current node and then delete the next node.

### 2. Code explained

- We store the next node in a temporary pointer `toDel`.
- We copy the value of the next node to the current node.
- We update the next pointer of the current node to the next of the next node.
- We delete the next node.

---

> This is a demonstration of Linked List manipulation.
