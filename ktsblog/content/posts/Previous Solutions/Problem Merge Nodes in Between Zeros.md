+++
title = 'Problem 2181 Merge Nodes in Between Zeros'
date = 2024-07-04T21:58:32+05:30
draft = false
series = 'leetcode'
tags =['linked-list','pointers']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2181](https://leetcode.com/problems/merge-nodes-in-between-zeros/description/)

## Question

You are given the `head` of a linked list, which contains a series of integers **separated** by `0`'s. The **beginning** and **end** of the linked list will have `Node.val == 0`.

For **every** two consecutive `0`'s, **merge** all the nodes lying in between them into a single node whose value is the sum of all the merged nodes. The modified list should not contain any `0`'s.

Return the `head` of the modified linked list.

## Example 1

{{<mermaid>}}
graph LR
A((0))-->B((3))
B-->C((1))
C-->D((0))
D-->E((4))
E-->F((5))
F-->G((2))
G-->H((0))
{{</mermaid>}}

```
Input: head = [0,3,1,0,4,5,2,0]
Output: [4,11]
Explanation:
The above figure represents the given linked list. The modified list contains
- The sum of the nodes marked in green: 3 + 1 = 4.
- The sum of the nodes marked in red: 4 + 5 + 2 = 11.
```

## Example 2

{{<mermaid>}}
graph LR
A((0))-->B((1))
B-->C((0))
C-->D((3))
D-->E((0))
E-->F((2))
F-->G((2))
G-->H((0))
{{</mermaid>}}

```
Input: head = [0,1,0,3,0,2,2,0]
Output: [1,3,4]
Explanation:
The above figure represents the given linked list. The modified list contains
- The sum of the nodes marked in green: 1 = 1.
- The sum of the nodes marked in red: 3 = 3.
- The sum of the nodes marked in yellow: 2 + 2 = 4.
```

## Constraints

```markdown
- The number of nodes in the list is in the range `[3, 2 * 10^5]`.
- `0 <= Node.val <= 1000`
- There are no two consecutive nodes with` Node.val == 0`.
- The beginning and end of the linked list have `Node.val == 0`.
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
    ListNode* mergeNodes(ListNode* head) {
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    ListNode* dummy = new ListNode(0);
    ListNode* prev = dummy;
    int sum;
    ListNode* curr = head;
    while (curr != nullptr) {
        sum = 0;
        while (curr != nullptr && curr->val == 0) {
            curr = curr->next;
        }

        while (curr != nullptr && curr->val != 0) {
            sum += curr->val;
            curr = curr->next;
        }

        if (sum != 0) {
            prev->next = new ListNode(sum);
            prev = prev->next;
        }
    }

    return dummy->next;
}

};
```

## Complexity Analysis

- `Time Complexity` : O(n)
- `Space Complexity` : O(1) excluding the space for the output list.

## Explanation

### 1. Intuition

```markdown
- We can use the zeroes as the delimiter to merge the nodes in between them.
- Use one pointer to traverse the list. If value is zero skip the current node
  and start adding the values of the nodes until the next zero is encountered.
- Once we encounter the next zero, we can add the sum of the nodes to the output list.
- Keep repeating the above steps until the end of the list is reached.
- Return the output list.
```

### 2. Implementation

```markdown
- Initialize a `dummy` node to 0.
- Initialize a `prev` pointer to `dummy`.
- Initialize a `sum` variable to 0.
- Initialize a `curr` pointer to `head`.
- Traverse the list until the end of the list is reached.
- While traversing the list
  1. Skip the current node till the value is zero.
  2. Add the values of the nodes until the next zero is encountered.
  3. If the sum is not zero then add the sum to the output list.
- Return the output list.
```
