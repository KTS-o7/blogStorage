+++
title = 'Problem Remove Nodes From Linked List'
date = 2024-05-06T15:21:47+05:30
draft = false
series = 'leetcode'
tags =['monotonic-stack','linked-list','pointers','stack','cpp']
toc = false
+++

# Problem Statement

**Link** - [Problem 2487](https://leetcode.com/problems/remove-linked-list-elements/description/)

## Question

You are given the `head` of a linked list.

Remove every node which has a node with a greater value anywhere to the right side of it.

Return the `head` of the modified linked list.

## Example 1

- Input
  {{<mermaid>}}
  graph LR
  A((5))-->B((2))
  B-->C((13))
  C-->D((3))
  D-->E((8))
  {{</mermaid>}}

- Output
  {{<mermaid>}}
  graph LR
  A((13))-->B((8))
  {{</mermaid>}}

```text
Input: head = [5,2,13,3,8]
Output: [13,8]
Explanation: The nodes that should be removed are 5, 2 and 3.
- Node 13 is to the right of node 5.
- Node 13 is to the right of node 2.
- Node 8 is to the right of node 3.
```

## Example 2

- Input
  {{<mermaid>}}
  graph LR
  A((1))-->B((1))
  B-->C((1))
  C-->D((1))
  {{</mermaid>}}

- Output
  {{<mermaid>}}
  graph LR
  A((1))-->B((1))
  B-->C((1))
  C-->D((1))
  {{</mermaid>}}

```text
Input: head = [1,1,1,1]
Output: [1,1,1,1]
Explanation: Every node has value 1, so no nodes are removed.
```

## Constraints

- The number of the nodes in the given list is in the range `[1, 10^5]`.
- `1 <= Node.val <= 10^5`

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
    ListNode* removeNodes(ListNode* head) {
        std::ios::sync_with_stdio(false);
        stack<ListNode*> st;
        ListNode* temp = head;
        while(temp != nullptr) {
            if(st.empty()) {
                st.push(temp);
                temp = temp->next;
            }
            else {
                while(!st.empty() && temp->val > st.top()->val)
                    st.pop();
                st.push(temp);
                temp=temp->next;
            }
        }

        ListNode* newHead = nullptr;
        while(!st.empty()) {
            temp  = st.top();
            st.pop();
            temp->next = newHead;
            newHead = temp;
        }

        return newHead;
    }
};
```

## Complexity

- `Time` : O(n)
- `Space` : O(n)

## Explanation

### 1. Intuition

- We want to remove every node which has a node with value greater than current node to the right side of it.
- If we use a loop once and check for every node to the right side of it, it will take `O(n^2)` time.
- We can use a stack to store the nodes in decreasing order.
- This monotonically decreasing stack will help us to remove the nodes which have a greater value to the right side of it.
- The idea is to traverse the linked list and keep pushing the nodes into the stack.
- If the current node has a greater value than the top of the stack, we will pop the stack until the top of the stack has a greater value than the current node.
- After traversing the linked list, we will pop the stack and create a new linked list in reverse order.
- We need it in reverse order because the stack will have the nodes from the end of the linked list to the start of the linked list.

### 2. Implementation

- We will create a stack to store the nodes.
- We will traverse the linked list and push the nodes into the stack.
- If the stack is empty, we will push the current node into the stack.
- If the stack is not empty, we will check if the current node has a greater value than the top of the stack.
- If the current node has a greater value than the top of the stack, we will pop the stack until the top of the stack has a greater value than the current node.
- After traversing the linked list, we will pop the stack and create a new linked list in reverse order.
- We will return the head of the new linked list.

---

> Note: This problem showcases the use of monotonic stack
