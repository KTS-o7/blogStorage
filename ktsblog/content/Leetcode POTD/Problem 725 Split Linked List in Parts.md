+++
title = 'Problem 725 Split Linked List in Parts'
date = 2024-09-08T14:29:52+05:30
draft = false
series = 'leetcode'
tags =['linked-list','pointers']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 725](https://leetcode.com/problems/split-linked-list-in-parts/description/)

## Question

Given the `head` of a singly linked list and an integer `k`, split the linked list into `k` consecutive linked list parts.

The length of each part should be as equal as possible: no two parts should have a size differing by more than one. This may lead to some parts being null.

The parts should be in the order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal to parts occurring later.

Return an array of the `k` parts.

## Example 1

![img1](https://assets.leetcode.com/uploads/2021/06/13/split1-lc.jpg)

```
Input: head = [1,2,3], k = 5
Output: [[1],[2],[3],[],[]]
Explanation:
The first element output[0] has output[0].val = 1, output[0].next = null.
The last element output[4] is null, but its string representation as a ListNode is [].
```

## Example 2

![img2](https://assets.leetcode.com/uploads/2021/06/13/split2-lc.jpg)

```
Input: head = [1,2,3,4,5,6,7,8,9,10], k = 3
Output: [[1,2,3,4],[5,6,7],[8,9,10]]
Explanation:
The input has been split into consecutive parts with size difference at most 1,
and earlier parts are a larger size than the later parts.
```

## Constraints

- The number of nodes in the list is in the range `[0, 1000]`.
- `0 <= Node.val <= 1000`
- `1 <= k <= 50`

## Solution

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptrptr) {}
 *     ListNode(int x) : val(x), next(nullptrptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    vector<ListNode*> splitListToParts(ListNode* head, int k)
    {
        ListNode* curr=head;
        int n=0;
        while(curr){
            curr=curr->next;
            n++;
        }
        int q = n/k;
        int r = n%k;

        vector<int> grplen(k, q);
        for (int i=0; i<r; i++)
            grplen[i]++;

        vector<ListNode*> ans(k);
        curr=head;

        for(int i=0; i<k; i++){
            ans[i]=curr;
            int j=0;
            ListNode* prev=nullptr;
            while(j<grplen[i]){
                prev=curr;
                curr=curr->next;
                j++;
            }
            if (prev)
                prev->next=nullptr;
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm                                | Time Complexity | Space Complexity  |
| ---------------------------------------- | --------------- | ----------------- |
| Counting the number of nodes in the list | O(n)            | O(1)              |
| Splitting the list into k parts          | O(n)            | O(k)O(n/k) = O(n) |
```

## Explanation

### 1. Intuition

Lets try to understand what the problem is asking

1. We need to split the linked list into `k` parts.
2. Each part must be independent of each other.
3. The length of each part should be as equal as possible.
4. The difference between the length of any two parts should not be more than 1.
5. The parts should be in the order of occurrence in the input list.
6. We need to use same nodes and not create new nodes.

Given this information, we can draw the following conclusions:

- First divide the total number of nodes in the list by `k` to get the number of minimum nodes in each part and how many nodes will be left.
- Then we can distribute the remaining nodes among the first `r` parts one at a time.
- So we need to keep track of the number of nodes in each part.
- Then we need to split the list into `k` parts and return the vector of parts.

Iterative idea to split a list into `2` parts:

```
First run a loop to count the number of nodes in the list.
ListNode* curr=head;
int n=0;
while(curr){
    curr=curr->next;
    n++;
}

int q = n/2;
int r = n%2;

vector<int> grplen(2, q);
if (r)
    grplen[0]++;

vector<ListNode*> ans(2);
curr=head;

for(int i=0; i<2; i++){
    ans[i]=curr;
    int j=0;
    ListNode* prev=nullptr;
    while(j<grplen[i]){
        prev=curr;
        curr=curr->next;
        j++;
    }
    if (prev)
        prev->next=nullptr;
}
```

In the above code we split the list into `2` parts.

> `prev` pointer will always point to the last node of the part.
> `curr` pointer will always point to the first node of the next part.
> Always push the `curr` pointer into the `ans` vector.
> then move `curr` and `prev` to next nodes until the length of the part is reached.
> If `prev` is not `nullptr` then make the `prev->next` as `nullptr`. This will split the list into parts.

### 2. Implementation

```markdown
- initialize `curr` to `head` and `n` to `0`.
- run a loop to count the number of nodes in the list.
- initialize `q` to `n/k` and `r` to `n%k`.
- initialize a vector `grplen` of size `k` and fill it with `q`.
- run a loop from `0` to `r` and increment the value of `grplen[i]`.
- initialize a vector `ans` of size `k`.
- for each index `i` from `0` to `k`:
  - push the `curr` into the `ans` vector.
  - initialize `j` to `0` and `prev` to `nullptr`.
  - run a loop until `j` is less than `grplen[i]`:
    - update `prev` to `curr`.
    - update `curr` to `curr->next`.
    - increment `j`.
  - if `prev` is not `nullptr` then make `prev->next` as `nullptr`.
- return the `ans` vector.
```
