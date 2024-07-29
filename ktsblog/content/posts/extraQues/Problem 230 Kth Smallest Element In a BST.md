+++
title = 'Problem 230 Kth Smallest Element in a BST'
date = 2024-07-29T23:09:53+05:30
draft = false
series = 'leetcode'
tags =['binary-search-tree','inorder-traversal','dfs']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 230](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

## Question

Given the `root` of a binary search tree, and an integer `k`, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

## Example 1

```
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

## Example 2

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

## Constraints

```markdown
- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 10^4`
- `0 <= Node.val <= 10^4`
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
    void solve(TreeNode* root, int &cnt, int &ans, int k){
        if(root == nullptr)
            return;
        //left, root, right
        solve(root->left, cnt, ans, k);
        cnt++;
        if(cnt == k){
            ans = root->val;
            return;
        }
        solve(root->right, cnt, ans, k);
    }
    int kthSmallest(TreeNode* root, int k) {
        std::ios::sync_with_stdio(false);
        int cnt = 0;
        int ans;
        solve(root, cnt, ans, k);
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm         | Time Complexity | Space Complexity |
| ----------------- | --------------- | ---------------- |
| Inorder Traversal | O(n)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- Inorder traversal of a BST gives the elements in sorted order.
- We can use this property to find the kth smallest element.
- We can do an inorder traversal and keep track of the count of elements visited.
- When the count becomes equal to k, we can return the element.
- We can use a recursive function to do this.
- We can do a left, root, right traversal.
```

### 2. Implementation

```markdown
- Define a recursive function `solve` which takes the root, count, answer, and k as arguments.
- if the `root` is `nullptr` then return.
- Call `solve` recursively for the left subtree.
- Increment the `count` by 1.
- If the `count` becomes equal to `k` then set the `answer` to the `root->val` and return.
- Call `solve` recursively for the right subtree.
```
