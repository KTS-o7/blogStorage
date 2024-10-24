+++
title = 'Problem 432 All O one Data Structure'
date = 2024-10-24T14:04:49+05:30
draft = false
series = 'leetcode'
tags =['data-structure','design','unordered-map','unordered-set','double-linked-list']
toc = false
math = true
+++

# Problem Statement

**Link** - [Problem 432](https://leetcode.com/problems/all-oone-data-structure/)

## Question

Design a data structure to store the strings' count with the ability to return the strings with minimum and maximum counts.

Implement the AllOne class:

- `AllOne()` Initializes the object of the data structure.
- `inc(String key)` Increments the count of the string key by 1. If key does not exist in the data structure, insert it with count 1.
- `dec(String key)` Decrements the count of the string key by 1. If the count of key is 0 after the decrement, remove it from the data structure. It is guaranteed that key exists in the data structure before the decrement.
- `getMaxKey()` Returns one of the keys with the maximal count. If no element exists, return an empty string `""`.
- `getMinKey()` Returns one of the keys with the minimum count. If no element exists, return an empty string `""`.

Note that each function must run in `O(1) average time complexity`.

## Example 1

```
Input
["AllOne", "inc", "inc", "getMaxKey", "getMinKey", "inc", "getMaxKey", "getMinKey"]
[[], ["hello"], ["hello"], [], [], ["leet"], [], []]
Output
[null, null, null, "hello", "hello", null, "hello", "leet"]

Explanation
AllOne allOne = new AllOne();
allOne.inc("hello");
allOne.inc("hello");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "hello"
allOne.inc("leet");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "leet"
```

## Constraints

- 1 <= `key.length` <= 10
- `key` consists of lowercase English letters.
- It is guaranteed that for each call to `dec`, `key` is existing in the data structure.
- At most 5`*`10<sup>4</sup> calls will be made to `inc`, `dec`, `getMaxKey`, and `getMinKey`.

## Solution

```cpp
class AllOne {
    struct Node {
        int count;
        unordered_set<string> keys;
        Node* prev;
        Node* next;
        Node(int c) : count(c), prev(nullptr), next(nullptr) {}
    };

    Node* head;
    Node* tail;
    unordered_map<string, Node*> key_to_node;

public:
    AllOne() {
        head = new Node(0);
        tail = new Node(0);
        head->next = tail;
        tail->prev = head;
    }

    void inc(string key) {
        if (key_to_node.find(key) == key_to_node.end()) {
            if (head->next->count == 1) {
                head->next->keys.insert(key);
                key_to_node[key] = head->next;
            } else {
                Node* newNode = new Node(1);
                newNode->keys.insert(key);
                newNode->next = head->next;
                newNode->prev = head;
                head->next->prev = newNode;
                head->next = newNode;
                key_to_node[key] = newNode;
            }
        } else {
            Node* curr = key_to_node[key];
            curr->keys.erase(key);
            int newCount = curr->count + 1;
            Node* next = curr->next;
            if (next->count == newCount) {
                next->keys.insert(key);
                key_to_node[key] = next;
            } else {
                Node* newNode = new Node(newCount);
                newNode->keys.insert(key);
                newNode->next = next;
                newNode->prev = curr;
                next->prev = newNode;
                curr->next = newNode;
                key_to_node[key] = newNode;
            }
            if (curr->keys.empty()) {
                curr->prev->next = curr->next;
                curr->next->prev = curr->prev;
                delete curr;
            }
        }
    }

    void dec(string key) {
        Node* curr = key_to_node[key];
        curr->keys.erase(key);
        int newCount = curr->count - 1;
        if (newCount == 0) {
            key_to_node.erase(key);
        } else {
            Node* prev = curr->prev;
            if (prev->count == newCount) {
                prev->keys.insert(key);
                key_to_node[key] = prev;
            } else {
                Node* newNode = new Node(newCount);
                newNode->keys.insert(key);
                newNode->next = curr;
                newNode->prev = prev;
                prev->next = newNode;
                curr->prev = newNode;
                key_to_node[key] = newNode;
            }
        }
        if (curr->keys.empty()) {
            curr->prev->next = curr->next;
            curr->next->prev = curr->prev;
            delete curr;
        }
    }

    string getMaxKey() {
        if (tail->prev == head) return "";
        return *(tail->prev->keys.begin());
    }

    string getMinKey() {
        if (head->next == tail) return "";
        return *(head->next->keys.begin());
    }
};

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne* obj = new AllOne();
 * obj->inc(key);
 * obj->dec(key);
 * string param_3 = obj->getMaxKey();
 * string param_4 = obj->getMinKey();
 */
```

## Complexity Analysis

```markdown
| Algorithm     | Time Complexity | Space Complexity |
| ------------- | --------------- | ---------------- |
| unordered_map | O(1)            | O(n)             |
| unordered_set | O(1)            | O(n)             |
| Total         | O(1)            | O(n)             |
```

## Explanation

### 1. Intuition: Multi-layered State Management System

#### Why this approach?

Think of this data structure like a chain of buckets, where:

- Each bucket (Node) represents a frequency count
- Inside each bucket, we store all strings that have that count
- Buckets are arranged in order of increasing frequency

For example, if we have:

- "hello" with count 2
- "world" with count 2
- "leetcode" with count 1

Our structure looks like this:

```
[head] <-> [count=1: {"leetcode"}] <-> [count=2: {"hello", "world"}] <-> [tail]
```

Why this design?

1. **Quick Access to Min/Max**:

   - The bucket next to head has the minimum frequency
   - The bucket before tail has the maximum frequency
   - This makes getMinKey() and getMaxKey() O(1) operations

2. **Efficient Increment/Decrement**:

   - When we increment "leetcode", we just move it from count-1 bucket to count bucket
   - If count bucket doesn't exist, we create it in the right position
   - All operations involve just updating a few pointers

3. **Smart Memory Management**:
   - We only create buckets for frequencies that actually exist
   - When a bucket becomes empty, we remove it
   - This keeps our chain as short as possible

### 2. Data Structure Visualization

```
key_to_node: {
    "hello" -> Node(count=2)
    "world" -> Node(count=2)
    "leetcode" -> Node(count=1)
}

Doubly-Linked List:
[head] <-> [Node1] <-> [Node2] <-> [tail]
where:
Node1 = {count: 1, keys: {"leetcode"}}
Node2 = {count: 2, keys: {"hello", "world"}}
```

The fundamental challenge in this problem is maintaining multiple ordered relationships while ensuring O(1) time complexity for all operations. The solution leverages a hybrid data structure that combines:

1. **Frequency Bucketing**:
   {{< katex >}}

   $$
   \begin{aligned}
   & \text{Let } F = \{f_1, f_2, ..., f_k\} \text{ be the set of frequencies} \\
   & \text{where } f_i < f_{i+1} \text{ for all } i \in [1, k-1]
   \end{aligned}
   $$

2. **Key-Value Association**:
   {{< katex >}}

   $$
   \text{Map: } K \rightarrow (F \times N) \text{ where K is the key space}
   $$

3. **Bidirectional Ordering**:
   {{< katex >}}
   $$
   \begin{aligned}
   & \text{For any frequency } f_i: \\
   & prev(f_i) = \max\{f_j \in F : f_j < f_i\} \\
   & next(f_i) = \min\{f_j \in F : f_j > f_i\}
   \end{aligned}
   $$

### 2. Data Structure Design

The structure consists of three main components:

1. **Frequency Node (Node)**:
   {{< katex >}}

   $$
   \text{Node} = \{
   \begin{cases}
   count &: \mathbb{N} \\
   keys &: \text{Set}<\text{string}> \\
   prev, next &: \text{Node*}
   \end{cases}
   \}
   $$

2. **Key Mapping**:
   {{< katex >}}

   $$
   \text{key\_to\_node}: \text{string} \rightarrow \text{Node*}
   $$

3. **Boundary Sentinels**:
   {{< katex >}}
   $$
   \begin{aligned}
   & head.count = 0, \text{ where } head.next \text{ points to min frequency} \\
   & tail.count = \infty, \text{ where } tail.prev \text{ points to max frequency}
   \end{aligned}
   $$

### 3. Operation Analysis

1. **Increment Operation (`inc`)**:
   {{< katex >}}

   $$
   \begin{aligned}
   & \text{For key } k \text{ with current frequency } f: \\
   & f' = f + 1 \\
   & \text{Node}_{new} =
   \begin{cases}
   \text{Node}_{f'} & \text{if } \exists \text{ node with count } f' \\
   \text{new Node}(f') & \text{otherwise}
   \end{cases}
   \end{aligned}
   $$

2. **Decrement Operation (`dec`)**:
   {{< katex >}}
   $$
   \begin{aligned}
   & \text{For key } k \text{ with current frequency } f: \\
   & f' = f - 1 \\
   & \text{Action} =
   \begin{cases}
   \text{delete key} & \text{if } f' = 0 \\
   \text{move to } \text{Node}_{f'} & \text{otherwise}
   \end{cases}
   \end{aligned}
   $$

### 4. Implementation Details

```cpp
class AllOne {
    struct Node {
        int count;
        unordered_set<string> keys;
        Node* prev;
        Node* next;
        Node(int c) : count(c), prev(nullptr), next(nullptr) {}
    };
```

Key Implementation Points:

1. **Node Insertion**:

```cpp
// Insert new node between prev and next
void insertNode(Node* newNode, Node* prev, Node* next) {
    newNode->next = next;
    newNode->prev = prev;
    prev->next = newNode;
    next->prev = newNode;
}
```

2. **Node Deletion**:

```cpp
// Remove node and reconnect links
void removeNode(Node* node) {
    node->prev->next = node->next;
    node->next->prev = node->prev;
    delete node;
}
```

3. **Key Movement**:

- When incrementing: Move key to next frequency bucket or create new bucket
- When decrementing: Move key to previous frequency bucket or delete if count becomes 0

### 5. Correctness Proof

1. **Invariants**:
   {{< katex >}}

   $$
   \begin{aligned}
   & I_1: \forall \text{ nodes } n_i, n_{i+1}: n_i.count < n_{i+1}.count \\
   & I_2: \forall \text{ keys } k: k \text{ exists in exactly one node} \\
   & I_3: head.count < \min(counts) \land max(counts) < tail.count
   \end{aligned}
   $$

2. **Time Complexity Proof**:
   {{< katex >}}
   $$
   \begin{aligned}
   & \text{For each operation op} \in \{\text{inc}, \text{dec}, \text{getMax}, \text{getMin}\}: \\
   & T(op) = O(1) \text{ as all operations involve}: \\
   & - \text{Constant number of pointer operations} \\
   & - \text{Hash table lookups/updates} \\
   & - \text{Set insertions/deletions}
   \end{aligned}
   $$

> **Technical Note:** The elegance of this solution lies in its ability to maintain both frequency ordering and key-value associations in O(1) time through careful structural design. The use of sentinel nodes (head/tail) simplifies boundary cases while maintaining the invariants necessary for correct operation.
