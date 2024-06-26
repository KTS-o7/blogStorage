+++
title = 'Problem Balance a Binary Search Tree'
date = 2024-06-26T14:23:32+05:30
draft = false
series = 'leetcode'
tags =['binary-search-tree','binary-tree','recursion','inorder']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 1382](https://leetcode.com/problems/balance-a-binary-search-tree/description/)

## Question

Given the `root` of a binary search tree, return a balanced binary search tree with the same node values. If there is more than one answer, **return any of them**.

A binary search tree is **balanced** if the depth of the two subtrees of every node never differs by more than `1`.

## Example 1

```text
Input: root = [1,null,2,null,3,null,4,null,null]
Output: [2,1,3,null,null,null,4]
Explanation: This is not the only correct answer, [3,1,4,null,2] is also correct.
```

#### Input

{{<mermaid>}}
graph TD
A((1)) --> B((2))
B --> C((3))
C--> D((4))
{{</mermaid>}}

#### Output

{{<mermaid>}}
graph TD
A((2)) --> B((1))
A --> C((3))
C --> D((4))
{{</mermaid>}}

## Example 2

```text
Input: root = [2,1,3]
Output: [2,1,3]
```

#### Input

{{<mermaid>}}
graph TD
A((2)) --> B((1))
A --> C((3))
{{</mermaid>}}

#### Output

{{<mermaid>}}
graph TD
A((2)) --> B((1))
A --> C((3))
{{</mermaid>}}

## Constraints

- The number of nodes in the tree is in the range `[1, 10^4]`.
- `1 <= Node.val <= 10^5`

## Solution

```cpp
class Solution {
public:
    TreeNode* balanceBST(TreeNode* root) {
        vector<int> sortedElements;
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);
        inOrderTraversal(root, sortedElements);
        return buildBalancedBST(sortedElements, 0, sortedElements.size() - 1);
    }

private:
    void inOrderTraversal(TreeNode* node, vector<int>& sortedElements) {
        if (!node) {
            return;
        }
        inOrderTraversal(node->left, sortedElements);
        sortedElements.push_back(node->val);
        inOrderTraversal(node->right, sortedElements);
    }

    TreeNode* buildBalancedBST(const vector<int>& elements, int start, int end) {
        if (start > end) {
            return nullptr;
        }
        int mid = start + (end - start) / 2;
        TreeNode* node = new TreeNode(elements[mid]);
        node->left = buildBalancedBST(elements, start, mid - 1);
        node->right = buildBalancedBST(elements, mid + 1, end);
        return node;
    }
};
```

## Complexity Analysis

- `Time Complexity` : O(n)
- `Space Complexity` : O(n)

## Explanation

### 1. Intuition

```markdown
- The given input is already sorted so if we perform an inorder traversal we will get the sorted elements.
- To build a balanced BST we need the root value to be from middle of the sorted elements.
- Tree can be recursively built by dividing the sorted elements into two halves.
```

### 2. Implementation

```markdown
- Using the `inOrderTraversal` function we can get the sorted elements.
- Using the `buildBalancedBST` function we can build the balanced BST.
- The `buildBalancedBST` function is a recursive function that takes the sorted elements and the start and end index.
- The mid value is calculated as the middle of the start and end index.
- The mid value is used as the root value and the left and right subtrees are recursively built.
- The base case is when the start index is greater than the end index.
- Then return `nullptr`.
```
