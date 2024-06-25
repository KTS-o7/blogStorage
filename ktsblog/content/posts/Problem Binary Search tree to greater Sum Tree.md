+++
title = 'Problem Binary Search Tree to Greater Sum Tree'
date = 2024-06-25T09:30:02+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','binary-search-tree','recursion','reverse-inorder']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1038](https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/description/)

## Question

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a binary search tree is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

#### Note

- The question is a bit confusing. Lets De-Leetcodify it.
- We need to convert the given Binary Search Tree to a Greater Sum Tree.
- In a Greater Sum Tree, every node's value is changed to the sum of all nodes greater than the node's value in the BST.
- To convert the BST to Greater Sum Tree, we need to do a reverse inorder traversal of the BST.

## Example 1

```text
Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

#### Input:

{{<mermaid>}}
graph TD
A((4)) --> B((1))
A --> C((6))
B --> D((0))
B --> E((2))
C --> F((5))
C --> G((7))
E --> H((3))
G --> I((8))
{{</mermaid>}}

#### Output:

{{<mermaid>}}
graph TD
A((30)) --> B((36))
A --> C((21))
B --> D((36))
B --> E((35))
C --> F((26))
C --> G((15))
E --> H((33))
G --> I((8))
{{</mermaid>}}

## Example 2

```text
Input: root = [0,null,1]
Output: [1,null,1]
```

#### Input:

{{<mermaid>}}
graph TD
A((0)) --> C((1))
{{</mermaid>}}

#### Output:

{{<mermaid>}}
graph TD
A((1)) --> C((1))
{{</mermaid>}}

## Constraints

- The number of nodes in the tree is in the range `[1, 100]`.
- `0 <= Node.val <= 100`
- All the values in the tree are **unique**.

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
    int sum = 0;
    TreeNode* helper(TreeNode* root)
    {
         if(root == nullptr)
            return root;
        helper(root->right);
        sum+=root->val;
        root->val=sum;
        helper(root->left);
        return root;
    }
    TreeNode* bstToGst(TreeNode* root) {
       std::ios::sync_with_stdio(false);
       cin.tie(nullptr);
       cout.tie(nullptr);
       return helper(root);
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(n)
- `Space Complexity` : O(n)

## Explanation

### 1. Intuition

```markdown
- To generate a Greater Sum Tree, we need to do a reverse inorder traversal of the BST.
- What is a reverse inorder traversal?
- In a reverse inorder traversal, we visit the right subtree first, then the root, and then the left subtree.
- We can use this to visit the right most leaf node first, then the parent node, and then the left child node.
- Now we can obtain the sum of all the nodes greater than the current node.
```

### 2. Implementation

```markdown
- Define a global variable `sum` to store the sum of all the nodes greater than the current node.
- Define a `helper` function that takes the `root` node as an argument.
- In the `helper` function, check if the `root` is `nullptr`, if so return the `root`.
- Recursively call the `helper` function with the right child of the `root`.
- Add the value of the `root` to the `sum`.
- Update the value of the `root` to the `sum`.
- Recursively call the `helper` function with the left child of the `root`.
- Return the `root`.
```

> This problem is exactly same as the [Problem 538](https://leetcode.com/problems/convert-bst-to-greater-tree/). The only difference is the name of the function. The problem statement is exactly the same. So, the solution is also the same.
