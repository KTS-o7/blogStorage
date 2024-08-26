+++
title = 'Problem 2096 Step by Step Directions From a Binary Tree Node to Another'
date = 2024-07-16T12:54:12+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','dfs','string']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2096](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/description/)

## Question

You are given the `root` of a **binary tree** with `n` nodes. Each node is **uniquely assigned** a value from `1` to `n`.
You are also given an integer `startValue` representing the value of the start node `s`, and a different integer `destValue` representing the value of the destination node `t`.

Find the **shortest path** starting from node `s` and ending at node `t`. Generate step-by-step directions of such path as a string consisting of only the **uppercase letters** `'L'`, `'R'`, and `'U'`. Each letter indicates a specific direction:

- `'L'` means to go from a node to its left child node.
- `'R'` means to go from a node to its right child node.
- `'U'` means to go from a node to its parent node.

Return the step-by-step directions of the **shortest path** from node `s` to node `t`.

## Example 1

```
Input: root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6
Output: "UURL"
Explanation: The shortest path is: 3 → 1 → 5 → 2 → 6.
```

## Example 2

```
Input: root = [2,1], startValue = 2, destValue = 1
Output: "L"
Explanation: The shortest path is: 2 → 1.
```

## Constraints

```markdown
- The number of nodes in the tree is `n`.
- `2 <= n <= 10^5`
- `1 <= Node.val <= n`
- All the values in the tree are `unique`.
- `1 <= startValue, destValue <= n`
- `startValue != destValue`
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
    TreeNode* LCA(TreeNode* root, int& p, int& q){
        if(root == nullptr || root->val == p || root->val == q)
            return root;
        TreeNode* left = LCA(root->left,p,q);
        TreeNode* right = LCA(root->right,p,q);
        if (left == nullptr)
            return right;
        if (right == nullptr)
            return left;
        return root;

    }

    bool dfs(TreeNode* root, int x, string& path, bool rev) {
        if (root == NULL)
            return 0;
        if (root->val == x)
            return 1;

        path += (rev ? 'U' : 'L');
        if (dfs(root->left, x, path, rev))
            return 1;
        path.pop_back();

        path += (rev ? 'U' : 'R');
        if (dfs(root->right, x, path, rev))
            return 1;
        path.pop_back();

        return 0;
    }

    string getDirections(TreeNode* root, int startValue, int destValue) {
        std::ios::sync_with_stdio(false);

        root = LCA(root, startValue, destValue);
        string pathFrom = "", pathTo = "";
        dfs(root,startValue,pathFrom,1);
        dfs(root,destValue,pathTo,0);
        return pathFrom + pathTo;
    }
};
```

## Complexity Analysis

```markdown
| Time Complexity | Space Complexity |
| --------------- | ---------------- |
| O(N)            | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- The idea is to find the Lowest Common Ancestor of the two nodes.
- This will help us to find the shortest path as the shortest path must pass through the LCA.
- Once we find the shortest path, we can find the directions from the start node to the LCA and from the LCA to the destination node.
- We need to build these paths as strings and return the concatenated string.
- How do we build a path ?
- Remember that from source to LCA it will always be `'U'` and `'L'` or `'R'` for LCA to destination.
- So we need to keep track of direction which we are finding the path.
- Use DFS, if we are going from LCA to Source append `'U'` else append `'L'` or `'R'` appropriately.
- We first try to append the direction to `path` and then try to find the path in the left subtree.
  If not found then we pop the last character from the path and append the direction to the right subtree.
  If not found then we pop the last character from the path.
```

### 2. Implementation

```markdown
- Define a function `LCA` to find the Lowest Common Ancestor of the two nodes.

1. `LCA`
   - If the `root` is `NULL` or the value of the root is equal to `p` or `q` return the root.
   - Recursively find the LCA in the left and right subtree.
   - If the left is `NULL` return right.
   - If the right is `NULL` return left.
   - Return the `root`.
2. DFS to find the path, this is a boolean function
   - If `root` is `NULL` return `0`. It means the path is not found.
   - If the value of the root is equal to `x` return `1`.
   - If `root` is not the target nor the leaf node then
     - Append `'U'` or `'L'` based on the `rev` flag.
     - Recursively find the path in the left subtree.
       - If the path is found return `1`.
     - Pop the last character from the path.
     - Append `'U'` or `'R'` based on the `rev` flag.
     - Recursively find the path in the right subtree.
       - If the path is found return `1`.
     - Pop the last character from the path.
   - Return `0`.

- In the main function `getDirections`
  - Find the LCA of the two nodes.
  - Initialize two strings `pathFrom` and `pathTo`.
  - Find the path from the source to the LCA and append it to `pathFrom`.
  - Find the path from the LCA to the destination and append it to `pathTo`.
  - Return the concatenated string of `pathFrom` and `pathTo`.
```
