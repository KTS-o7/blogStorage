+++
title = 'Problem 1325 Delete Leaves With a Given Value'
date = 2024-05-19T10:27:59+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','dfs','recursion','postorder']
toc = false
+++

# Problem Statement

**Link** - [Problem 1325](https://leetcode.com/problems/delete-leaves-with-a-given-value/description/)

## Question

Given a binary tree `root` and an integer `target`, _delete all the leaf nodes_ with value `target`.

Note that once you delete a leaf node with value `target`, if its parent node becomes a leaf node and has the value `target`, it should also be deleted (you need to continue doing that until you cannot).

## Example 1

```text
Input: root = [1,2,3,2,null,2,4], target = 2
Output: [1,null,3,null,4]
Explanation: Leaf nodes in green with value (target = 2) are removed (Picture in left).
After removing, new nodes become leaf nodes with value (target = 2) (Picture in center).
```

{{<mermaid>}}
flowchart TD
id1(input)
A((1)) --> B((2))
A --> C((3))
C --> D((2))
D --> E((2))
D --> F((4))
id2(Output)
G((1)) -->H((3))
H --> I((4))
{{</mermaid>}}

## Example 2

```text
Input: root = [1,3,3,3,2], target = 3
Output: [1,3,null,null,2]
```

{{<mermaid>}}
flowchart TD
id1(input)
A((1)) --> B((3))
A --> C((3))
C --> D((3))
D --> E((3))
D --> F((2))
id2(Output)
G((1)) -->H((3))
H --> I((2))
{{</mermaid>}}

## Example 3

```text
Input: root = [1,2,null,2,null,2], target = 2
Output: [1]
Explanation: Leaf nodes in green with value (target = 2) are removed at each step.
```

{{<mermaid>}}
flowchart TD
id1(input)
A((1)) --> B((2))
A --> C((2))
C --> D((2))
id2(Output)
E((1))
{{</mermaid>}}

## Constraints

- The number of nodes in the tree is in the range `[1, 3000]`.
- `1 <= Node.val, target <= 1000`

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
    TreeNode* removeLeafNodes(TreeNode* root, int target) {
        if (!root)
            return nullptr;

        root->left = removeLeafNodes(root->left, target);
        root->right = removeLeafNodes(root->right, target);

        if (!root->left && !root->right && root->val == target)
            return nullptr;

        return root;

    }
};
```

## Complexity Analysis

- `Time complexity` - O(N)
- `Space complexity` - O(H)

## Explanation

### 1. Intuition

- This can be solved using DFS.
- We will traverse the tree in post-order fashion.
- If the node is leaf and has the value equal to the target, Remove it.
- Otherwise, return the node.

### 2. Implementation

- If the `root` is null, return null.
- Recursively call the function on the left and right subtree.
- By this we will reach the leaf nodes.
- If the leaf node has the value equal to the target, return null.
- Otherwise, return the node.

---
