+++
title = 'Problem 1367 Linked List in Binary Tree'
date = 2024-09-07T13:48:11+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','linked-list','dfs','recursion']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem ](https://leetcode.com/problems/linked-list-in-binary-tree/description/)

## Question

Given a binary tree `root` and a linked list with `head` as the first node.

Return True if all the elements in the linked list starting from the `head` correspond to some downward path connected in the binary tree otherwise return False.

## Example 1

![img1](https://assets.leetcode.com/uploads/2020/02/12/sample_1_1720.png)

```
Input: head = [4,2,8],
       root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
Output: true
Explanation: Nodes in blue form a subpath in the binary Tree.
```

## Example 2

![img2](https://assets.leetcode.com/uploads/2020/02/12/sample_2_1720.png)

```
Input: head = [1,4,2,6],
       root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
Output: true
```

## Example 3

```
Input: head = [1,4,2,6,8],
       root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
Output: false
Explanation: There is no path in the binary tree containing all the elements of the linked list from head.
```

## Constraints

- The number of nodes in the tree will be in the range `[1, 2500]`.
- The number of nodes in the list will be in the range `[1, 100]`.
- `1 <= Node.val <= 100` for each node in the linked list and binary tree

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
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool dfs(ListNode* head, TreeNode* root){
        if(head == nullptr)
            return true;
        if(root==nullptr)
            return false;
        if(root->val != head->val)
            return false;
        return dfs(head->next,root->left) || dfs(head->next,root->right);
    }
    bool isSubPath(ListNode* head, TreeNode* root) {
        if(root == nullptr)
            return false;
        if(dfs(head,root))
            return true;
        else
            return isSubPath(head,root->left) || isSubPath(head,root->right);
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity              | Space Complexity             |
| --------- | ---------------------------- | ---------------------------- |
| DFS       | O(list.size() x tree.size()) | O(list.size() + tree.size()) |
| isSubPath | O(list.size() x tree.size()) | O(list.size() + tree.size()) |
```

## Explanation

### 1. Intuition

Let's try to breakdown the meaning of the problem.
We need to find if the linked list is present in the binary tree or not.
Conditions to check:

1. If list is empty it is always present in the tree.
2. If tree is empty and list is not empty then it is not present in the tree.
3. A list may be starting from any node in the tree.
4. If we get the starting node of the list in the tree then we need to check if the list is present in the tree or not.

Using the above conditions few points to be noted:

- We need to check depth wise if the list is present in the tree or not. Hence we need to use DFS.
- We need to check recursively if the list is present in the left or right subtree of the tree.

Possible edge cases:

1. If the tree is empty then the list is not present in the tree. Hence return false.
2. If the list is empty then the list is always present in the tree. Hence return true.
3. If the list is not empty and tree is not empty then we need to recursively check if the list is present in the tree or not.

`dfs` function:
We can use this function to check if the list is present starting from a given tree node.

- If the list is empty then it is always present in the tree. Hence return true.
- If the tree node is empty then the list is not present in the tree. Hence return false.
- If the value of the tree node is not equal to the value of the list node then list wont be starting from this node. Hence return false.
- If it satisfies the above conditions then it means the current tree node is representing the current list node. Hence we need to check if the next list node is present in either left or right subtree of the current tree node.

`isSubPath` function:
We can use this function to find the possible starting node of the list in the tree.

- If the tree is empty then the list is not present in the tree. Hence return false.
- If the DFS function returns true then it means the list is present in the tree. Hence return true.
- If the DFS function returns false then we need to recursively check if the starting node of the list is present in the left or right subtree of the tree.

### 2. Implementation

```markdown
- `dfs` function is used to check if the list is present starting from a given tree node.
- Input: listnode, treenode
- Output: boolean
- Base case: If listnode is empty then return true.
- Base case: If treenode is empty then return false.
- Base case: If treenode value is not equal to listnode value then return false.
- Recursive case: If the above conditions are not satisfied then check if the `next` listnode is present in
  either left or right subtree of the current treenode.

- `isSubPath` function is used to find the possible starting node of the list in the tree.
- Input: listnode, treenode
- Output: boolean
- Base case: If treenode is empty then return false.
- Base case: If dfs function returns true then return true. Indicates the list is present in the tree.
- Recursive case: If the dfs function returns false then check if the starting node of the list is present in
  either left or right subtree of the current treenode.
```
