+++
title = 'Problem Find the Minimum and Maximum Number of Nodes Between Critical Points'
date = 2024-07-05T18:54:28+05:30
draft = false
series = 'leetcode'
tags =['linked-list','pointers','math']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2058](https://leetcode.com/problems/find-the-minimum-and-maximum-number-of-nodes-between-critical-points/description/)

## Question

A **critical point** in a linked list is defined as either a **local maxima** or a **local minima**.

A node is a **local maxima** if the current node has a value **strictly greater** than the previous node and the next node.

A node is a **local minima** if the current node has a value **strictly smaller** than the previous node and the next node.

Note that a node can only be a **local maxima/minima** if there exists both a previous node and a next node.

Given a linked list `head`, return an array of length 2 containing `[minDistance, maxDistance]` where `minDistance` is the **minimum distance between any two distinct critical points** and `maxDistance` is the **maximum distance between any two distinct critical points**. If there are fewer than two critical points, return `[-1, -1]`.

## Example 1

{{<mermaid>}}
graph LR
A((3)) --> B((1))
{{</mermaid>}}

```
Input: head = [3,1]
Output: [-1,-1]
Explanation: There are no critical points in [3,1].
```

## Example 2

{{<mermaid>}}
graph LR
A((5)) --> B((3))
B --> C((1))
C --> D((2))
D --> E((5))
E --> F((1))
F --> G((2))
{{</mermaid>}}

```
Input: head = [5,3,1,2,5,1,2]
Output: [1,3]
Explanation: There are three critical points:
- [5,3,1,2,5,1,2]: The third node is a local minima because 1 is less than 3 and 2.
- [5,3,1,2,5,1,2]: The fifth node is a local maxima because 5 is greater than 2 and 1.
- [5,3,1,2,5,1,2]: The sixth node is a local minima because 1 is less than 5 and 2.
The minimum distance is between the fifth and the sixth node. minDistance = 6 - 5 = 1.
The maximum distance is between the third and the sixth node. maxDistance = 6 - 3 = 3.

```

## Example 3

{{<mermaid>}}
graph LR
A((1)) --> B((3))
B --> C((2))
C --> D((2))
D --> E((3))
E --> F((2))
F --> G((2))
G --> H((2))
H --> I((7))
{{</mermaid>}}

```
Input: head = [1,3,2,2,3,2,2,2,7]
Output: [3,3]
Explanation: There are two critical points:
- [1,3,2,2,3,2,2,2,7]: The second node is a local maxima because 3 is greater than 1 and 2.
- [1,3,2,2,3,2,2,2,7]: The fifth node is a local maxima because 3 is greater than 2 and 2.
Both the minimum and maximum distances are between the second and the fifth node.
Thus, minDistance and maxDistance is 5 - 2 = 3.
Note that the last node is not considered a local maxima because it does not have a next node.
```

## Constraints

```markdown
- The number of nodes in the list is in the range `[2, 10^5]`.
- `1 <= Node.val <= 10^5`
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
    vector<int> nodesBetweenCriticalPoints(ListNode* head) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        ListNode* prev = head;
        ListNode* curr = head->next;

        if(head->next == nullptr)
            return {-1,-1};

        vector<int>positions;
        int ind = 1;
        while(curr->next!=nullptr)
        {
            if((prev->val<curr->val && curr->val>curr->next->val)||(prev->val>curr->val && curr->val<curr->next->val))
            {
                positions.push_back(ind);
            }
            ind++;
            prev = curr;
            curr =curr->next;
        }
        if(positions.size()<2)
            return {-1,-1};

        int mini = INT_MAX;

        for(int i = 1;i<positions.size();i++)
            mini = min(mini,positions[i]-positions[i-1]);

        return {mini,positions.back() - positions.front()};

    }
};
```

## Complexity Analysis

- `Time Complexity` : O(n)
- `Space Complexity` : O(1) excluding the space for the output list.

## Explanation

### 1. Intuition

```markdown
- We need to find the critical points in the linked list and then find the minimum and maximum distance between them.
- That means first we have to find the indexes of the critical points.
- Then we can find the minimum and maximum distance between them.
- We can find the critical points by comparing the current node with the previous and the next node.
- Create an ascending sequence of the indexes of the critical points.
- Maximum will be the difference between the last and the first index.
- Minimum will be the minimum difference between the indexes.
- Edge cases are when there are no critical points or only one critical point.
- When there is less than 3 nodes in the linked list, there will be no critical points.
```

### 2. Implementation

```markdown
- Delcare two pointers `prev` and `curr` to traverse the linked list.
- `prev` will point to the `head` and `curr` will point to the next node.
- If there are less than 2 nodes in the linked list, return `[-1,-1]`.
- Declare a vector `positions` to store the indexes of the critical points.
- Declare an integer `ind` to store the index of the current node, initialize it to 1.
- While `curr->next` is not `nullptr`
  1. If the current node is a critical point, add the index to the `positions` vector.
  2. Increment the index `ind`.
  3. Move the `prev` and `curr` pointers to the next node.
- If there are less than 2 critical points, return `[-1,-1]`.
- Initialize an integer `mini` to `INT_MAX`.
- Iterate over the `positions` vector from the second element to the last element.
  1. Find the minimum difference between the indexes of the critical points.
- Return the minimum and maximum distance between the critical points.
```
