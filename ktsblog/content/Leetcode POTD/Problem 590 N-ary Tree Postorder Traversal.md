+++
title = 'Problem 590 N Ary Tree Postorder Traversal'
date = 2024-08-26T10:27:00+05:30
draft = false
series = 'leetcode'
tags =['tree','postorder']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 590](https://leetcode.com/problems/n-ary-tree-postorder-traversal/)

## Question

Given the `root` of an n-ary tree, return the postorder traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

## Example 1

![img1](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [5,6,3,2,4,1]
```

## Example 2

![img2](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [2,6,14,11,7,3,12,8,4,13,9,10,5,1]
```

## Constraints

```markdown
- `The number of nodes in the tree is in the range [0, 10^4].`
- `0 <= Node.val <= 10^4`
- `The height of the n-ary tree is less than or equal to 1000`.
```

#### NOTE

Reminder:

- PREorder: ROOT|Left|Right
- INorder: Left|ROOT|Right
- POSTorder: Left|Right|ROOT
- In case of an N-ary Tree, POSTorder is: Child1|Child2|...|ChildN|ROOT

## Solution

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    void traverse(Node* root, vector<int>& v){
        if(!root)
            return;
        for(auto it:root->children)
                traverse(it,v);
        v.push_back(root->val);
    }
    vector<int> postorder(Node* root) {
        vector<int> ans;
        traverse(root,ans);
        return ans;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm           | Time Complexity | Space Complexity |
| ------------------- | --------------- | ---------------- |
| Postorder Traversal | O(N)            | O(N)             |
```

## Explanation

### 1. Intuition

```markdown
- Idea is to visit all the children of a node first and then visit the node itself.
- So iterate over the children and call the function recursively.
- Finally, push the value of the node into the vector.
```

### 2. Implementation

```markdown
- Intialize a vector `ans` to store the postorder traversal.
- Call the `traverse` function with the root and the vector.
- Return `ans`.

- In `traverse` function:
  - If the root is NULL, return.
  - Iterate over the children of the root and call the `traverse` function recursively.
  - Push the value of the root into the vector.
```
