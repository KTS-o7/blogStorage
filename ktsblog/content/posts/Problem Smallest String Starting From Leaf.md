+++
title = 'Problem Smallest String Starting From Leaf'
date = 2024-04-17T14:56:36+05:30
draft = false
series = 'leetcode'
tags = ['binary tree', 'dfs']
toc = false
+++

# Problem Statement

**Link** - [Problem 988](https://leetcode.com/problems/smallest-string-starting-from-leaf/description/)

## Question

You are given the `root` of a binary tree where each node has a value in the range `[0, 25]` representing the letters `'a' to 'z'`.

Return the `lexicographically` smallest string that starts at a `leaf` of this tree and ends at the `root`.

As a reminder, any shorter prefix of a string is lexicographically smaller.

For example, `"ab"` is lexicographically smaller than `"aba"`.
A leaf of a node is a node that has no children.

## Example 1

{{<mermaid>}}
graph TD
A[0] --> B[1]
A --> C[2]
B --> D[3]
B --> E[4]
C --> F[3]
C --> G[4]
{{</mermaid>}}

```text
Input: root = [0,1,2,3,4,3,4]
Output: "dba"
```

## Example 2

{{<mermaid>}}
graph TD
A[25] --> B[1]
B --> C[1]
B --> D[3]
A --> E[3]
E --> F[0]
E --> G[2]
{{</mermaid>}}

```text
Input: root = [25,1,3,1,3,0,2]
Output: "adz"
```

## Example 3

{{<mermaid>}}
graph TD
A[2] --> B[2]
A --> C[1]
B --> D[1]
C--> E[0]
D-->F[0]
{{</mermaid>}}

```text
Input: root = [2,2,1,null,1,0,null,0]
Output: "abc"
```

## Important Edge Case

{{<mermaid>}}
graph TD
A[0] --> B[1]
{{</mermaid>}}

```text
Input: root =  [0,null,1]
Output: "ba"
```

## Constraints

- The number of nodes in the tree is in the range `[1, 8500]`.
- `0` <= Node.val <= `25`

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
    void dfs(TreeNode* root, string str, string& smallest)
    {
        if(root == NULL)
            return;

        str+= char('a'+root->val);
        if(root->left == NULL && root->right == NULL)
        {
            reverse(str.begin(),str.end());
            if(smallest.empty() || str<smallest)
                smallest = str;
            reverse(str.begin(),str.end());
        }

        dfs(root->left,str,smallest);
        dfs(root->right,str,smallest);


    }
    string smallestFromLeaf(TreeNode* root) {
        string small;
        dfs(root,"",small);
        return small;
    }
};
```

## Complexity

- `Time : O(N)`
- `Space : O(N)`

## Explanation

1. We perform a `DFS` traversal of the tree.
2. First we start from the `root` of the tree and traverse down.
3. We use a string called `smallest` to keep track of the smallest string from leafnode to rootnode found so far.
4. **Base Case** for recursion - if root is `NULL` then just return.
5. We append the character to the string `str` which is the character at the current node.
6. String `str` actually stores the current path which is being developed from rootnode to leafnode.
7. If we reach a leafnode then we reverse the `str` and check if its the lexographically smallest string which we saw so far.
8. If yes then update the `smallest`.
9. Then reverse `str` again so that we can continue exploring the path.
10. Then we recursively explore the left subtree and right subtree looking for leaf nodes.

---

> This solution effectively demonstrates the usage of DFS to build a string from a tree following the given set of rules.
