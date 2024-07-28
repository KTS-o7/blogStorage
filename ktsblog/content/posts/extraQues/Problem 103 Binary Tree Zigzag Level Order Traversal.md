+++
title = 'Problem 103 Binary Tree Zigzag Level Order Traversal'
date = 2024-07-29T00:31:56+05:30
draft = false
series = 'leetcode'
tags =[]
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 103](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)

## Question

Given the `root` of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).

## Example 1

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
```

## Example 2

```
Input: root = [1]
Output: [[1]]
```

## Example 3

```
Input: root = []
Output: []
```

## Constraints

```markdown
- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if(root==nullptr)
            return {};
        vector<vector<int>>ans;
        queue<TreeNode*>q;
        q.push(root);
        bool flag = true;
        while(!q.empty()){
            int size = q.size();
            vector<int>v;

            for(int i =0 ;i<size;i++){
                TreeNode* t = q.front();
                q.pop();
                if(t->left)
                    q.push(t->left);
                if(t->right)
                    q.push(t->right);
                if(flag){
                    v.push_back(t->val);
                }
                else{
                    v.insert(v.begin(),t->val);
                }
            }
            ans.push_back(v);
            flag =!flag;
        }
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm       | Time Complexity | Space Complexity |
| --------------- | --------------- | ---------------- |
| BFS level order | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition

```markdown
- The logic is follows.
- We will do just BFS, but maintain a flag to check if we need to reverse the order of the elements in the vector.
```

### 2. Implementation

```markdown
- If the root is `nullptr` return empty vector.
- Create a vector of vector of int `ans`.
- Create a queue of TreeNode\* `q`.
- Push the root into the queue.
- Create a flag `flag` to check if we need to reverse the order of the elements in the vector.
- Until the queue is not empty
  - Get the size of the queue.
  - Create a vector of int `v`.
  - Loop through the size of the queue
    - Get the front element of the queue.
    - Pop the front element of the queue.
    - If the left of the front element is not `nullptr` push it into the queue.
    - If the right of the front element is not `nullptr` push it into the queue.
    - If the flag is true push the value of the front element into the vector.
    - Else insert the value of the front element at the beginning of the vector.
  - Push the vector into the `ans`.
  - Reverse the flag.
- Return the `ans`.
```
