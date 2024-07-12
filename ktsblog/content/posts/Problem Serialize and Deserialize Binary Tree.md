+++
title = 'Problem Serialize and Deserialize Binary Tree'
date = 2024-07-12T19:24:33+05:30
draft = false
series = 'leetcode'
tags =['binary-tree','string','queue','level-order-traversal','bfs']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)

## Question

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

## Example 1

![image](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

## Example 2

```
Input: root = []
Output: []

```

## Constraints

```markdown
- The number of nodes in the tree is in the range `[0, 10^4]`.
- `-1000 <= Node.val <= 1000`
```

## Solution

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Codec {
public:
    string serialize(TreeNode* root) {
        string ans = "";
        TreeNode* temp = root;
        queue<TreeNode*>q;
        q.push(temp);
        while(!q.empty())
        {
            for(int i = 0; i<q.size(); i++)
            {
                if(q.front() != nullptr)
                {
                    ans+=to_string(q.front()->val);
                    ans+='#';
                    q.push(q.front()->left);
                    q.push(q.front()->right);
                }
                else
                {
                    ans+="null#";
                }
                q.pop();
            }
        }
        return ans;

    }

TreeNode* deserialize(string data) {
    vector<string> nodes;
    int l = 0, r = 0;
    while (l < data.size()) {
        while (r < data.size() && data[r] != '#') {
            r++;
        }
        nodes.push_back(data.substr(l, r - l));
        l = r + 1;
        r = l;
    }

    if(nodes[0] == "null")
        return nullptr;

    TreeNode* root = new TreeNode(stoi(nodes[0]));
    queue<TreeNode*>q;
    q.push(root);
    int ind = 1;

    while(!q.empty())
    {
        TreeNode* curr = q.front();
        q.pop();
        if(nodes[ind]!="null")
        {
            TreeNode* temp = new TreeNode(stoi(nodes[ind]));
            curr->left = temp;
            q.push(temp);
        }
        else
        {
            curr->left = nullptr;
        }
        ind++;

        if(nodes[ind]!="null")
        {
            TreeNode* temp = new TreeNode(stoi(nodes[ind]));
            curr->right = temp;
            q.push(temp);
        }
        else
        {
            curr->right = nullptr;
        }
        ind++;
    }
    return root;
}

};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```

## Complexity Analysis

- `Time Complexity` : O(n) where n is the number of nodes in the tree.
- `Space Complexity` : O(n) where n is the number of nodes in the tree.

## Explanation

### 1. Intuition

```markdown
1. Serialization :
   - We can serialize a binary tree using level order traversal.
   - Use a queue to store the nodes of the tree.
   - If the node is not null, push the value of the node to the string and push the left and right child of the node to the queue.
   - If the node is null, push "null" to the string.
   - Use an appropriate delimiter to separate the values of the nodes.
2. Deserialization :
   - Split the string using the delimiter and store the values in a vector.
   - Create a queue and push the root node to the queue.
   - Iterate over the vector and create the left and right child of the current node.
   - Push the left and right child to the queue.
   - Return the root node.
```

### 2. Implementation

```markdown
1. Serialization :
   - Initialize a string `ans` and a queue `q`.
   - Push the root node to the queue.
   - Iterate over the queue and check if the front of the queue is not null.
   - If the front of the queue is not null, push the value of the node to the string and push the left and right child of the node to the queue.
   - append the delimiter `#` to the string.
   - If the front of the queue is null, push "`null#`" to the string.
   - Return the string.
2. Deserialization :
   - Split the string using the delimiter `#` and store the values in a vector `nodes`.
   - If the first element of the vector is `"null"`, return `nullptr`.
   - Create a root node with the value of the first element of the vector.
   - Create a queue `q` and push the root node to the queue.
   - Initialize an index `ind` to 1.
   - Iterate over the queue and create the left and right child of the current node in the following manner
     - If the value at index `ind` is not `"null"`, create a new node with the value and assign it as the left child of the current node.
     - Push the left child to the queue.
     - Increment the index `ind`.
     - If the value at index `ind` is not `"null"`, create a new node with the value and assign it as the right child of the current node.
     - Push the right child to the queue.
     - Increment the index `ind`.
   - Return the root node.
```

---

> Picture credits : LeetCode
