+++
title = 'Problem Evaluate Boolean Binary Tree'
date = 2024-05-17T21:05:30+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','dfs','bit-manipulation','cpp']
toc = false
+++

# Problem Statement

**Link** - [Problem 2331](https://leetcode.com/problems/evaluate-boolean-binary-tree/description/)

## Question

You are given the `root` of a **full binary tree** with the following properties:

- **Leaf nodes** have either the value `0` or `1`, where `0` represents `False` and `1` represents `True`.
- **Non-leaf nodes** have either the value `2` or `3`, where `2` represents the boolean `OR` and `3` represents the boolean `AND`.

The `evaluation` of a node is as follows:

- If the node is a leaf node, the evaluation is the value of the node, i.e. True or False.
- Otherwise, **evaluate** the node's two children and apply the boolean operation of its value with the children's evaluations.
  Return the boolean result of **evaluating** the `root` node.

A _full binary tree_ is a binary tree where each node has either `0` or `2` children.

A _leaf node_ is a node that has zero children.

## Example 1

Input
{{<mermaid>}}
graph TD
A((2))-->B((1))
A-->C((3))
C((3))-->D((0))
C((3))-->E((1))
F((OR))-->G((True))
F-->H((AND))
H-->I((False))
H-->J((True))

{{</mermaid>}}

```text
Input: root = [2,1,3,null,null,0,1]
Output: true
Explanation: The above diagram illustrates the evaluation process.
The AND node evaluates to False AND True = False.
The OR node evaluates to True OR False = True.
The root node evaluates to True, so we return true.
```

## Example 2

```text
Input: root = [0]
Output: false
Explanation: The root node is a leaf node and it evaluates to false, so we return false.
```

## Constraints

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 3`
- Every node has either `0` or `2` children.
- Leaf nodes have a value of `0` or `1`.
- Non-leaf nodes have a value of `2` or `3`.

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
    bool evaluateTree(TreeNode* root) {
        if(root->left == nullptr && root->right == nullptr)
            return root->val;

        else
        {
            if(root->val == 2)
                return evaluateTree(root->left) || evaluateTree(root->right);
            else
                return  evaluateTree(root->left) && evaluateTree(root->right);
        }

    }
};
```

## Complexity Analysis

- `Time`: O(N)
- `Space`: O(N)

## Explanation

### 1. Intuition

- To evalute any node we need to evaluate its left and right child.
- There is no need to evaluate the leaf nodes as they are the base case.
- Using this we can develop a DFS based solution.

### 2. Implementation

- The `evaluateTree` function is used to evaluate the tree.
- If the node is a leaf node, we return the value of the node.
- Otherwise, we evaluate the left and right child and apply the operation based on the value of the node.
- If the value of the node is 2, we apply OR operation.
- If the value of the node is 3, we apply AND operation.
