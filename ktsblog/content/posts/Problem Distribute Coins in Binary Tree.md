+++
title = 'Problem Distribute Coins in Binary Tree'
date = 2024-05-19T10:29:15+05:30
draft = false
series = 'leetcode'
tags =['dfs','recursion','postorder','binary-tree']
toc = false
+++

# Problem Statement

**Link** - [Problem 979](https://leetcode.com/problems/distribute-coins-in-binary-tree/description/)

## Question

You are given the `root` of a binary tree with `n` nodes where each node in the tree has `node.val` coins. There are n coins in total throughout the whole tree.

In one move, we may choose two adjacent nodes and move one coin from one node to another. A move may be from parent to child, or from child to parent.

Return the minimum number of moves required to make every node have exactly one coin.

### Note

- Actually here the word minimum doesnt make any sense.
- Because we can move coins from parent to child and child to parent.
- What exactly we need to find is the flow of coins in a given path.

## Example 1

```text
Input: root = [3,0,0]
Output: 2
Explanation: From the root of the tree, we move one coin to its left child, and one coin to its right child.
```

## Example 2

```text
Input: root = [0,3,0]
Output: 3
Explanation: From the left child of the root, we move two coins to the root [taking two moves]. Then, we move one coin from the root of the tree to the right child.
```

## Example 3 (Important case)

Since the only constraint is number of coins in the tree is same as number of nodes, it can be distributed in any manner.

```text
Input: root = [1,0,2]
Output: 2
Explanation: From the root of the tree, we move one coin to the left child.
             From right child, we move one coin to the root.
```

## Constraints

- The number of nodes in the tree is `n`.
- `1 <= n <= 100`
- `0 <= node.val <= n`
- The sum of all `node.val` is `n`.

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
    int countSteps(TreeNode* root, int& step)
    {
        if( root == nullptr)
            return 0;
        int leftCoin = countSteps(root->left,step);
        int rightCoin = countSteps(root->right,step);
        step += abs(leftCoin) + abs(rightCoin);
        return (root->val -1) + leftCoin + rightCoin;
    }

public:
    int distributeCoins(TreeNode* root) {
        int ans = 0;
        countSteps(root,ans);
        return ans;

    }
};
```

## Complexity Analysis

- `Time Complexity` - O(N) - We are visiting each node once.
- `Space Complexity` - O(H) - Height of the tree.

## Explanation

### 1. Intuition

- We just need to find the movement of coins in the tree.
- If a node has `y` coins, then it will give `y-1` coins to its parent.
- If a node has `0` coins then it will request `1` coin from its parent.
- So, we can say that `abs(y-1)` is the number of coins that will be moved from that node.
- Number of coins moved from a node is `abs(z-1)` + Coins from left subtree + Coins from right subtree.

### 2. Implementation

- If the `root` is null, return 0. This handles the base case.
- Recursively call the function on the left and right subtree.
- Calculate the number of coins moved from the left and right subtree. Store it in `leftCoin` and `rightCoin`.
- Store total steps so far done in `step`.
- Calculate the number of coins moved from the current node using the formula `root->val - 1 + leftCoin + rightCoin`.

---
