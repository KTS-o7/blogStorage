+++
title = 'Problem 623 Add One Row to Tree'
date = 2024-04-16T10:05:52+05:30
draft = false
series = 'leetcode'
tags = ['cpp','binary-tree','dfs']
toc = false
+++

# Problem Statement

Link - [Problem 623](https://leetcode.com/problems/add-one-row-to-tree/description/)

## Question

Given the `root` of a binary tree and two integers val and depth, add a row of nodes with value val at the given depth depth.

Note that the `root` node is at depth `1`.

The adding rule is:

- Given the integer `depth`, for each not null tree node `cur` at the depth `depth - 1`, create two tree nodes with value val as `cur's` left subtree root and right subtree root.
- `cur's` original left subtree should be the left subtree of the new left subtree root.
- `cur's` original right subtree should be the right subtree of the new right subtree root.
- If `depth == 1` that means there is no depth `depth - 1` at all, then create a tree node with value val as the new root of the whole original tree, and the original tree is the new root's left subtree.

## Example 1

```text
Input: root = [4,2,6,3,1,5], val = 1, depth = 2
Output: [4,1,1,2,null,null,6,3,1,5]
```

# Example 2

```text
Input: root = [4,2,null,3,1], val = 1, depth = 3
Output: [4,2,null,1,1,3,null,null,1]
```

## Constraints

```text
The number of nodes in the tree is in the range [1, 10^4].
The depth of the tree is in the range [1, 10^4].
-100 <= Node.val <= 100
-10^5 <= val <= 10^5
1 <= depth <= the depth of tree + 1
```

## Solution

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(NULL), right(NULL) {}
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root,int val,int currDepth,int depth)
    {
        if(root == NULL)
            return;
        if(currDepth == depth-1)
        {
            root->left = new TreeNode(val,root->left,NULL);
            root->right = new TreeNode(val,NULL,root->right);
            return;
        }

        dfs(root->left,val,currDepth+1,depth);
        dfs(root->right,val,currDepth+1,depth);
    }
    TreeNode* addOneRow(TreeNode* root, int val, int depth) {
        if(depth == 1)
            {
             return new TreeNode(val,root,NULL);
            }

        dfs(root,val,1,depth);
        return root;

    }
};
```

## Complexity

- `Time : O(N)`
- `Space : O(N)`

## Explaination

The provided code is a C++ solution for adding a new row of nodes with a given value to a binary tree at a specified depth. The solution uses a depth-first search (DFS) approach to traverse the tree and add the new nodes. Here's a breakdown of the approach and logic:

## Approach

1. **Depth-First Search (DFS)**: The solution uses a DFS approach to traverse the binary tree. DFS is chosen because it allows us to explore each path from the root to a leaf node in a systematic manner, which is crucial for adding nodes at a specific depth.

2. **Adding Nodes at the Specified Depth**: The `dfs` function is designed to add new nodes at the specified depth. It takes four parameters: the current node (`root`), the value to be added to the new nodes (`val`), the current depth (`currDepth`), and the target depth (`depth`).

3. **Base Case**: The base case for the DFS is when the current depth equals the target depth minus one. At this point, the function creates new nodes with the given value and adds them as the left and right children of the current node. This effectively adds a new row of nodes at the specified depth.

4. **Recursive Calls**: For each node, the function makes recursive calls to itself for the left and right children of the current node, incrementing the current depth by one. This allows the function to explore all nodes at the specified depth and add the new nodes.

5. **Handling the Root Node**: If the target depth is 1, the `addOneRow` function creates a new root node with the given value and the original root as its right child. This effectively adds a new row of nodes at the root level.

## Logic

- **DFS Traversal**: The DFS traversal starts from the root and explores each path from the root to a leaf node. The traversal is controlled by the `currDepth` and `depth` parameters, ensuring that nodes are added only at the specified depth.

- **Adding New Nodes**: When the DFS reaches the specified depth, it adds new nodes with the given value as the left and right children of the current node. This is done by creating new `TreeNode` instances with the given value and the existing left and right children of the current node.

- **Returning the Modified Tree**: After adding the new nodes, the `addOneRow` function returns the modified tree. If the target depth is 1, it returns a new root node; otherwise, it returns the original root node.

## Why This Approach Works ?

- **Systematic Exploration**: DFS ensures that we explore all nodes in the tree in a systematic manner, allowing us to add new nodes at the specified depth.

- **Depth Control**: By comparing `currDepth` with `depth - 1`, we can precisely control when to add new nodes, ensuring that they are added only at the specified depth.

- **Flexibility**: This approach can handle adding a new row of nodes at any depth within the tree, making it versatile for different scenarios.

> This solution effectively demonstrates how to use DFS to modify a binary tree by adding a new row of nodes at a specified depth.
