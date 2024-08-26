+++
title = 'Problem 2816 Double a Number Represented as a Linked List'
date = 2024-05-07T18:14:06+05:30
draft = false
series = 'leetcode'
tags =['linked-list','cpp','stack','pointers']
toc = false
+++

# Problem Statement

**Link** - [Problem 2816](https://leetcode.com/problems/double-a-number-represented-as-a-linked-list/description)

## Question

You are given the `head` of a _non-empty linked list_ representing a **non-negative integer without leading zeroes**.

Return the `head` of the linked list after doubling it

## Example 1

- Input
  {{<mermaid>}}
  graph LR
  A((1))-->B((8))
  B-->C((9))
  {{</mermaid>}}

- Output
  {{<mermaid>}}
  graph LR
  A((3))-->B((7))
  B-->C((8))
  {{</mermaid>}}

```text
Input: head = [1,8,9]
Output: [3,7,8]
Explanation: The figure above corresponds to the given linked list which represents the number 189.
            Hence, the returned linked list represents the number 189 * 2 = 378.
```

## Example 2

- Input
  {{<mermaid>}}
  graph LR
  A((9))-->B((9))
  B-->C((9))
  {{</mermaid>}}

- Output
  {{<mermaid>}}
  graph LR
  A((1))-->B((9))
  B-->C((9))
  C-->D((8))
  {{</mermaid>}}

```text
Input: head = [9,9,9]
Output: [1,9,9,8]
Explanation: The figure above corresponds to the given linked list which represents the number 999.
             Hence, the returned linked list reprersents the number 999 * 2 = 1998.
```

## Constraints

- The number of nodes in the list is in the range `[1, 10^4]`.
- `0 <= Node.val <= 9`
- The input is generated such that the list represents a number that does not have leading zeros, except the number `0` itself.

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
    ListNode* doubleIt(ListNode* head) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        stack<ListNode*>st;
        ListNode* temp = head;
        while(temp!=nullptr)
        {
            st.push(temp);
            temp=temp->next;
        }

        int carry = 0, newVal = 0;
        while(!st.empty())
        {
            temp = st.top();
            st.pop();
            newVal = 2*temp->val + carry;
            carry = newVal/10;
            newVal = newVal%10;
            //cout<<carry<<newVal<<endl;
            temp->val = newVal;
        }
        if(carry)
        {
            ListNode* newhead = new ListNode(carry,head);
            return newhead;
        }

        return head;
    }
};
```

## Complexity

- `Time` : O(n)
- `Space` : O(n)

## Explanation

### 1. Intuition

- We need to iterate the linked list in reverse order to double the number.
- From the last node we need to double the value and keep track of the carry.
- We can use a stack to store the nodes in reverse order.

### 2. Implementation

- Have a stack to store the nodes in reverse order.
- Push all the nodes into the stack.
- Pop the nodes from the stack and double the value of the node.
- Calculate the carry and the new value.
- Carry = NewValue/10, NewValue = NewValue%10.
- Update the value of the node.
- Do this for all the nodes.
- If there is a carry left after the last node, create a new node with the carry and return the new head.
- Else return the head of the linked list.

---

> This is a problem where FIFO principle of stack is used to reverse the linkedlist without manipulating any pointers.
