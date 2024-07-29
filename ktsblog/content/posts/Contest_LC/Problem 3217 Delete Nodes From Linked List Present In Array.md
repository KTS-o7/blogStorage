+++
title = 'Problem 3217 Delete Nodes From Linked List Present in Array'
date = 2024-07-29T19:41:13+05:30
draft = false
series = 'leetcode'
tags =['linked-list','pointers']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 3217](https://leetcode.com/problems/delete-nodes-from-linked-list-present-in-array/description/)

## Question

You are given an array of integers `nums` and the `head` of a linked list. Return the `head` of the modified linked list after removing all nodes from the linked list that have a value that exists in `nums`.

## Example 1

```
Input: nums = [1,2,3], head = [1,2,3,4,5]

Output: [4,5]
```

## Example 2

```
Input: nums = [1], head = [1,2,1,2,1,2]

Output: [2,2,2]
```

## Example 3

```
Input: nums = [5], head = [1,2,3,4]

Output: [1,2,3,4]
```

## Constraints

```markdown
- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^5`
- All elements in `nums` are unique.
- `The number of nodes in the given list is in the range [1, 10^5]`.
- `1 <= Node.val <= 10^5`
- The input is generated such that there is at least one node in the linked list that has a value not present in `nums`.
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
    ListNode* modifiedList(vector<int>& nums, ListNode* head) {
       unordered_set<int> s(nums.begin(),nums.end());

        ListNode * ans=head;

        while(ans!=nullptr && s.find(ans->val)!=s.end()){
            ans=ans->next;
        }

        head=ans;

        while(head!=nullptr && head->next!=nullptr){
            if(s.find(head->next->val)!=s.end()){
                head->next=head->next->next;
            }
            else{
                head=head->next;
            }
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm         | Time Complexity | Space Complexity |
| ----------------- | --------------- | ---------------- |
| Pointer Traversal | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition

```markdown
- Use a hash set to store the elements of the array.
- This will be used for lookup.
- Initialize a pointer to the head of the linked list.
- Traverse the linked list and remove the nodes which are present in the hash set.
```

### 2. Implementation

```markdown
- Initialize a hash set `s` with the elements of the array.
- Initialize a pointer `ans` to the head of the linked list.
- While `ans` is not null and the value of `ans` is present in the hash set, move `ans` to the next node.
- Update the head of the linked list to `ans`.
- While the head of the linked list is not null and the next node of the head is not null:
  - If the value of the next node of the head is present in the hash set, `head->next` will point to the next node of the next node.
  - Else move the head to the next node.
- Return the head of the linked list.
```
