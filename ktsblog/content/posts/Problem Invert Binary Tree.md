+++
title = 'Problem Invert Binary Tree'
date = 2024-06-30T18:59:42+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','tree','recursion']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 226](https://leetcode.com/problems/invert-binary-tree/description/)

## Question

Given the `root` of a binary tree, invert the tree and return its root.

## Example 1

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

#### Input:

{{<mermaid>}}
graph TD
4 --- 2
4 --- 7
2 --- 1
2 --- 3
7 --- 6
7 --- 9
{{</mermaid>}}

#### Output :

{{<mermaid>}}
graph TD
4 --- 7
4 --- 2
7 --- 9
7 --- 6
2 --- 3
2 --- 1
{{</mermaid>}}

## Example 2

```
Input: root = [2,1,3]
Output: [2,3,1]
```

#### Input :

{{<mermaid>}}
graph TD
2 --- 1
2 --- 3
{{</mermaid>}}

#### Output :

{{<mermaid>}}
graph TD
2 --- 3
2 --- 1
{{</mermaid>}}

## Example 3

```
Input: root = []
Output: []
```

## Constraints

```markdown
- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`
```

## Solution

```cpp
/*
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
    void invert(TreeNode* root)
    {
        if(root)
        {
            swap(root->left,root->right);
            invert(root->left);
            invert(root->right);
        }
        else
            return;
    }
    TreeNode* invertTree(TreeNode* root) {
        invert(root);
        return root;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(N)
- `Space Complexity` : O(N)

## Explanation

### 1. Intuition

```markdown
- We need to produce a mirror image of the binary tree.
- To do this we just need to swap the left and right subtrees recursively.
```

### 2. Implementation

```markdown
- Define a helper function `invert` which takes a `TreeNode*` as input.
- If the root is not null, swap the left and right subtrees.
- Recursively call the `invert` function on the left and right subtrees.
- Return the root.
```
