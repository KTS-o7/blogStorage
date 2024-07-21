+++
title = 'Problem Sum Root to Leaf Numbers'
date = 2024-04-15T19:38:03+05:30
draft = false
series = 'leetcode'
tags = ['cpp','binary-tree','dfs']
toc = false
+++

# Problem Statement

Link - [Problem 129](https://leetcode.com/problems/sum-root-to-leaf-numbers/description/)

## Question

You are given the root of a binary tree containing digits from 0 to 9 only.

Each root-to-leaf path in the tree represents a number.

- For example, the root-to-leaf path `1 -> 2 -> 3` represents the number 123.

Return the total sum of all root-to-leaf numbers. Test cases are generated so that the answer will fit in a 32-bit integer.

A leaf node is a node with no children.

## Example 1

```
Input: root = [1,2,3]
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```

## Example 2

```
Input: root = [4,9,0,5,1]
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

## Constraints

```
Constraints:

The number of nodes in the tree is in the range [1, 1000].
0 <= Node.val <= 9
The depth of the tree will not exceed 10.

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
    int dfs(TreeNode* root, int path)
    {
        if(root==nullptr)
            return 0;
        path = path*10 + root->val;
        if(root->left == nullptr && root->right == nullptr)
            return path;

        return dfs(root->left,path)+dfs(root->right,path);
    }
    int sumNumbers(TreeNode* root) {
        return dfs(root,0);
    }
};

```

## Complexity

- `Time : O(N)`
- `Space : O(N)`

## Explanation

1. Depth-First Search (DFS): The solution uses a depth-first search (DFS) approach to traverse the binary tree. DFS is chosen because it allows us to explore each path from the root to a leaf node in a systematic manner.
2. Path Representation: As we traverse the tree, we keep track of the current path from the root to the current node. This path is represented as an integer, where each digit in the path corresponds to the value of a node in the path.
3. Appending Node Values: For each node we visit, we append its value to the current path. This is done by multiplying the current path by 10 (to shift the digits to the left) and adding the node's value. This operation effectively appends the node's value to the end of the path.
4. Base Case: The base case for the DFS is when we reach a leaf node (a node with no children). At this point, the path represents a number, and we return this number.
5. Recursive Calls: For each non-leaf node, we make recursive calls to the DFS function for its left and right children, passing along the updated path. This allows us to explore all possible paths from the root to the leaves.
6. Summing Numbers: The sum of all numbers represented by the paths from the root to the leaves is calculated by adding up the numbers returned by the recursive calls.

## Why this technique works ?

- Systematic Exploration: DFS ensures that we explore all possible paths from the root to the leaves in a systematic manner. This is crucial for calculating the sum of all numbers represented by these paths.
- Path Representation: By representing the path as an integer, we can easily append the value of each node to the path and calculate the number represented by the path. This representation is efficient and straightforward.
- Base Case Handling: The base case (reaching a leaf node) allows us to calculate the number represented by the path from the root to the leaf. This is the key step in solving the problem.
- Recursive Summing: By making recursive calls and summing the results, we ensure that the sum of all numbers represented by the paths from the root to the leaves is calculated correctly.

> This technique works because it systematically explores all paths from the root to the leaves of the binary tree, calculates the number represented by each path, and sums these numbers to find the total sum of all root-to-leaf numbers.
