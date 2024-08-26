+++
title = 'Problem 2196 Create Binary Tree From Descriptions'
date = 2024-07-15T15:17:40+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','hashmap']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 2196](https://leetcode.com/problems/create-binary-tree-from-descriptions/description/)

## Question

You are given a 2D integer array `descriptions` where d`escriptions[i] = [parenti, childi, isLefti]` indicates that` parenti` is the parent of `childi` in a binary tree of **unique values**. Furthermore,

If `isLefti == 1`, then `childi` is the left child of` parenti`.
If `isLefti == 0`, then `childi` is the right child of `parenti`.
Construct the binary tree described by `descriptions` and return its root.

The test cases will be generated such that the binary tree is valid.

## Example 1

{{<mermaid>}}
graph TD
A((50)) --> B((20))
A --> C((80))
B --> D((15))
B --> E((17))
C --> F((19))
{{</mermaid>}}

```
Input: descriptions = [[20,15,1],[20,17,0],[50,20,1],[50,80,0],[80,19,1]]
Output: [50,20,80,15,17,19]
Explanation: The root node is the node with value 50 since it has no parent.
The resulting binary tree is shown in the diagram.
```

## Example 2

{{<mermaid>}}
graph TD
A((1)) --> B((2))
B((2)) --> C((3))
C((3)) --> D((4))
{{</mermaid>}}

```
Input: descriptions = [[1,2,1],[2,3,0],[3,4,1]]
Output: [1,2,null,null,3,4]
Explanation: The root node is the node with value 1 since it has no parent.
The resulting binary tree is shown in the diagram.
```

## Constraints

```markdown
- `1 <= descriptions.length <= 10^4`
- `descriptions[i].length == 3`
- `1 <= parenti, childi <= 10^5`
- ` 0 <= isLefti <= 1`
- The binary tree described by `descriptions` is valid.
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
    TreeNode* createBinaryTree(vector<vector<int>>& descriptions) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        unordered_map<int,TreeNode*> mp;
        unordered_set<int>isChild;

        for(auto &it:descriptions){
            if(mp.find(it[0]) == mp.end()){
                mp[it[0]] = new TreeNode(it[0]);
            }
            if(mp.find(it[1])==mp.end()){
                mp[it[1]] = new TreeNode(it[1]);
            }
            if(it[2]){
                mp[it[0]]->left = mp[it[1]];
            }
            else{
                mp[it[0]]->right = mp[it[1]];
            }
            isChild.insert(it[1]);
        }

        TreeNode* root = nullptr;
        for(auto &it : descriptions){
            if(isChild.find(it[0]) == isChild.end()){
                root = mp[it[0]];
                return root;
            }
        }
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
- Root node will be the node which has no parent.
- So we need to maintain a set of nodes which are child nodes.
- First create a map of all nodes and form the tree.
- To find the root node, iterate over the set of nodes and find the node which is not a child node.
```

### 2. Implementation

```markdown
- Initialize a map `mp` to store the nodes.
- Initialize a set `isChild` to store the child nodes.
- Traverse the `descriptions` array.
- If the parent node is not present in the map, then create a new node and add it to the map.
- If the child node is not present in the map, then create a new node and add it to the map.
- If the child node is a left child, then add the child node to the left of the parent node.
- If the child node is a right child, then add the child node to the right of the parent node.
- Add the child node to the set of child nodes.
- Finally, iterate over the `descriptions` array.
- Find the node which is not a child node.
- Return the root node.
```

> The key idea is to find the node which is not a child node and return it as the root node.
