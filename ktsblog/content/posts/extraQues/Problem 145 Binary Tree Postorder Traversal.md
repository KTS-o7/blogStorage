+++
title = 'Problem 145 Binary Tree Postorder Traversal'
date = 2024-08-26T21:43:05+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','postorder','recursive']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 145](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)

## Question

Given the `root` of a binary tree, return the postorder traversal of its nodes' values.

## Example 1

```
Input: root = [1,null,2,3]
Output: [3,2,1]
```

## Example 2

```
Input: root = []
Output: []
```

## Example 3

```
Input: root = [1]
Output: [1]
```

## Constraints

```markdown
- `The number of the nodes in the tree is in the range [0, 100].`
- `-100 <= Node.val <= 100`
```

## Solution

```cpp
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
    void postorder(TreeNode* root, vector<int>& v){
        if(root){
            postorder(root->left,v);
            postorder(root->right,v);
            v.push_back(root->val);
        }
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int>v;
        postorder(root,v);
        return v;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm           | Time Complexity | Space Complexity |
| ------------------- | --------------- | ---------------- |
| Postorder Traversal | O(N)            | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- We need to recursive visit the left and right child of the root node
  and then visit the root node.
```

### 2. Implementation

```markdown
- Initialize a vector `v` to store the postorder traversal of the binary tree.
- Write a recursive function `postorder` that takes
  the root node and the vector `v` as arguments.
- If the root node is not null, then recursively call the `postorder` function
  for the left child of the root node
  and then for the right child of the root node.
- After visiting the left and right child, push the value of the root node into the vector `v`.
- Finally, return the vector `v` containing the postorder traversal of the binary tree.
```
