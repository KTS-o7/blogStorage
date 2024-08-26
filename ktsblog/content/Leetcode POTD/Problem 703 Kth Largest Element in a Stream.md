+++
title = 'Problem 703 Kth Largest Element in a Stream'
date = 2024-08-12T10:08:40+05:30
draft = false
series = 'leetcode'
tags =['priority-queue','heap','design','class']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 703](https://leetcode.com/problems/kth-largest-element-in-a-stream/description/)

## Question

Design a class to find the `kth` largest element in a stream. Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Implement KthLargest class:

- `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of integers `nums`.
- `int add(int val)` Appends the integer `val` to the stream and returns the element representing the `kth` largest element in the stream.

## Example 1

```
Input
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
Output
[null, 4, 5, 5, 8, 8]

Explanation
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3);   // return 4
kthLargest.add(5);   // return 5
kthLargest.add(10);  // return 5
kthLargest.add(9);   // return 8
kthLargest.add(4);   // return 8
```

## Constraints

```markdown
- `1 <= k <= 10^4`
- `0 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `-10^4 <= val <= 10^4`
- At most `10^4` calls will be made to `add`.
- It is guaranteed that there will be at least `k` elements in the array when you search for the `kth` element.
```

## Solution

```cpp
class KthLargest {
public:
    priority_queue<int, vector<int>, greater<int>> pq;
    int len;
    KthLargest(int k, vector<int> nums) {
        len=k;
        for(auto val:nums) {
            pq.push(val);
            if(pq.size()>k)
                pq.pop();
        }
    }

    int add(int val) {
        pq.push(val);
        if(pq.size()>len)
            pq.pop();
        return pq.top();
    }
};
/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Heaping   | O(nlogk)        | O(k)             |
```

## Explanation

### 1. Intuition

```markdown
- We need to keep track of first `k` largest elements in the stream.
- Hence we can use a min heap to store the `k` largest elements.
- Since we are storing only `k` largest elements, when the size of the heap exceeds `k`, we can pop the smallest element.
- When the size exceeds `k` we need to pop because they are `k+1th`, `k+2th` elements etc.
```

### 2. Implementation

```markdown
- Initialize a priority queue `pq` with a min heap.
- Initialize a variable `len` to store the value of `k`.
- In the constructor, iterate over the `nums` and push the elements into the heap.
  - If the size of the heap exceeds `k`, pop the smallest element.
- In the `add` function, push the element into the heap.
  - If the size of the heap exceeds `k`, pop the smallest element.
  - Return the top element of the heap.
```
