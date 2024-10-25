+++
title = 'Problem 1381 Design a Stack With Increment Operation'
date = 2024-10-25T22:49:59+05:30
draft = false
series = 'leetcode'
tags =['stack','design','array']
toc = false
math = true
+++

# Problem Statement

**Link** - [Problem 1381](https://leetcode.com/problems/design-a-stack-with-increment-operation/description/)

## Question

Design a stack that supports increment operations on its elements.

Implement the `CustomStack` class:

- `CustomStack(int maxSize)` Initializes the object with `maxSize` which is the maximum number of elements in the stack.
- `void push(int x)` Adds x to the top of the stack if the stack has not reached the `maxSize`.
- `int pop()` Pops and returns the top of the stack or `-1` if the stack is empty.
- `void inc(int k, int val)` Increments the bottom `k` elements of the stack by `val`. If there are less than `k` elements in the stack, increment all the elements in the stack.

## Example 1

```
Input
["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]
Output
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
Explanation
CustomStack stk = new CustomStack(3); // Stack is Empty []
stk.push(1);                          // stack becomes [1]
stk.push(2);                          // stack becomes [1, 2]
stk.pop();                            // return 2 --> Return top of the stack 2, stack becomes [1]
stk.push(2);                          // stack becomes [1, 2]
stk.push(3);                          // stack becomes [1, 2, 3]
stk.push(4);                          // stack still [1, 2, 3], Do not add another elements as size is 4
stk.increment(5, 100);                // stack becomes [101, 102, 103]
stk.increment(2, 100);                // stack becomes [201, 202, 103]
stk.pop();                            // return 103 --> Return top of the stack 103, stack becomes [201, 202]
stk.pop();                            // return 202 --> Return top of the stack 202, stack becomes [201]
stk.pop();                            // return 201 --> Return top of the stack 201, stack becomes []
stk.pop();                            // return -1 --> Stack is empty return -1.
```

## Constraints

- 1 <= `maxSize, x, k` <= 1000
- 0 <= `val` <= 100
- At most `1000` calls will be made to each method of increment, push and pop each separately.

## Solution

```cpp
class CustomStack {
private:

    vector<int> v;
    int top;
    int size;

public:
    CustomStack(int maxSize) {
        v.resize(maxSize);
        top = -1;
        size = maxSize-1;
    }

    void push(int x) {
        if (top < size) {
            v[++top] = x;
        }
    }

    int pop() {
        if (top >= 0) {
            return v[top--];
        }
        return -1;
    }

    void increment(int k, int val) {
        int limit = min(k, top + 1);
        for (int i = 0; i < limit; i++) {
            v[i] += val;
        }
    }
};

/**
 * Your CustomStack object will be instantiated and called as such:
 * CustomStack* obj = new CustomStack(maxSize);
 * obj->push(x);
 * int param_2 = obj->pop();
 * obj->increment(k,val);
 */
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Push      | O(1)            | O(1)             |
| Pop       | O(1)            | O(1)             |
| Increment | O(k)            | O(1)             |
| Storing   | O(n)            | O(n)             |
```

## Explanation

### 1. Intuition: Stack with Enhanced Functionality

#### Why this approach?

The core challenge of this problem is to implement a stack with an additional increment operation while maintaining optimal time complexity. Let's break down the key insights:

1. **Bounded Stack Operations**:

   - Traditional push/pop operations need to respect a maximum size
   - Stack needs constant time access to the top element
   - Need efficient way to track number of elements

2. **Increment Operation Challenge**:

   - Need to modify bottom k elements
   - k could be larger than actual stack size
   - Need to handle partial increments efficiently

3. **Design Choices**:
   ```
   [1, 2, 3] (maxSize = 3)
   increment(2, 100) → [101, 102, 3]
   ```
   Using an array-based implementation provides:
   - O(1) push/pop operations
   - Direct access to elements for increment
   - Easy size management

### 2. Mathematical Foundation

1. **Stack State Representation**:
   {{< katex >}}

   $$
   \begin{aligned}
   & S = [a_1, a_2, ..., a_n] \text{ where } n \leq maxSize \\
   & top = \text{index of last element} = n-1 \\
   & \text{Valid indices: } i \in [0, top]
   \end{aligned}
   $$

2. **Operation Definitions**:
   {{< katex >}}
   $$
   \begin{aligned}
   & push(x): S_{new} =
   \begin{cases}
   [a_1, ..., a_n, x] & \text{if } n < maxSize \\
   [a_1, ..., a_n] & \text{otherwise}
   \end{cases} \\
   & pop(): \text{return }
   \begin{cases}
   a_n \text{ and } S_{new} = [a_1, ..., a_{n-1}] & \text{if } n > 0 \\
   -1 & \text{otherwise}
   \end{cases} \\
   & increment(k, val): a_i =
   \begin{cases}
   a_i + val & \text{if } i < min(k, n) \\
   a_i & \text{otherwise}
   \end{cases}
   \end{aligned}
   $$

### 3. Implementation Details

1. **Data Structure Design**:

```cpp
class CustomStack {
private:
    vector<int> v;     // Stack array
    int top;           // Current top index
    int size;          // Maximum size - 1
};
```

2. **Key Methods Implementation**:

a) **Constructor**:

```cpp
CustomStack(int maxSize) {
    v.resize(maxSize);  // Preallocate space
    top = -1;          // Empty stack indicator
    size = maxSize-1;  // Store max index
}
```

b) **Push Operation**:

```cpp
void push(int x) {
    if (top < size) {      // Check space available
        v[++top] = x;      // Increment top and add element
    }
}
```

c) **Pop Operation**:

```cpp
int pop() {
    if (top >= 0) {        // Check if stack not empty
        return v[top--];   // Return and decrement top
    }
    return -1;             // Empty stack case
}
```

d) **Increment Operation**:

```cpp
void increment(int k, int val) {
    int limit = min(k, top + 1);  // Calculate actual elements to increment
    for (int i = 0; i < limit; i++) {
        v[i] += val;              // Increment elements
    }
}
```

### 4. Complexity Analysis

1. **Time Complexity**:
   {{< katex >}}

   $$
   \begin{aligned}
   & T_{push} = O(1) \\
   & T_{pop} = O(1) \\
   & T_{increment} = O(min(k, n)) \text{ where n is current stack size}
   \end{aligned}
   $$

2. **Space Complexity**:
   {{< katex >}}
   $$
   S_{total} = O(maxSize) \text{ for storing the stack array}
   $$

### 5. Correctness Proof

### 1. Formal Invariants Definition

Let's first formally define what it means for our stack to be in a valid state. We maintain three critical invariants:

#### Invariant 1 (I₁): Valid Top Index Range

```
-1 ≤ top ≤ size where size = maxSize - 1
```

- top = -1 represents empty stack
- top ≤ size ensures we never exceed maxSize

#### Invariant 2 (I₂): Valid Element Access

```
∀i ∈ [0, top]: v[i] contains a valid stack element
```

- All elements from index 0 to top are valid stack elements
- Elements beyond top are undefined

#### Invariant 3 (I₃): Stack Size Consistency

```
current_stack_size = top + 1
```

- Maintains relationship between top index and actual number of elements

### 2. Initialization Proof

We need to prove the constructor establishes all invariants:

```cpp
CustomStack(int maxSize) {
    v.resize(maxSize);
    top = -1;
    size = maxSize-1;
}
```

**Proof of Initial State:**

1. I₁ holds: top = -1, and -1 ≤ -1 ≤ size
2. I₂ holds vacuously: there are no elements in range [0, -1]
3. I₃ holds: stack_size = 0 = -1 + 1

### 3. Operation Preservation Proofs

#### 3.1 Push Operation

```cpp
void push(int x) {
    if (top < size) {
        v[++top] = x;
    }
}
```

**Proof of Push:**

1. I₁ preservation:

   - If top < size:
     - New top = old_top + 1
     - Since old_top < size, new_top ≤ size
   - If top = size:
     - top remains unchanged
       Therefore, -1 ≤ top ≤ size remains true

2. I₂ preservation:

   - If push occurs:
     - All previous elements [0, old_top] remain valid
     - New element at top + 1 is explicitly set
   - If push is skipped:
     - All elements remain unchanged

3. I₃ preservation:
   - If push occurs:
     - new_size = old_size + 1
     - new_top = old_top + 1
   - If push is skipped:
     - Both size and top remain unchanged

#### 3.2 Pop Operation

```cpp
int pop() {
    if (top >= 0) {
        return v[top--];
    }
    return -1;
}
```

**Proof of Pop:**

1. I₁ preservation:

   - If top ≥ 0:
     - New top = old_top - 1
     - Since old_top ≤ size, new_top < size
   - If top < 0:
     - top remains -1
       Therefore, -1 ≤ top ≤ size remains true

2. I₂ preservation:

   - If pop occurs:
     - All elements [0, new_top] remain valid
   - If pop is skipped:
     - No elements exist (valid for empty stack)

3. I₃ preservation:
   - If pop occurs:
     - new_size = old_size - 1
     - new_top = old_top - 1
   - If pop is skipped:
     - Both remain at 0

#### 3.3 Increment Operation

```cpp
void increment(int k, int val) {
    int limit = min(k, top + 1);
    for (int i = 0; i < limit; i++) {
        v[i] += val;
    }
}
```

**Proof of Increment:**

1. I₁ preservation:

   - top remains unchanged
   - Therefore, -1 ≤ top ≤ size remains true

2. I₂ preservation:

   - Only modifies values, not structure
   - Only accesses elements in range [0, min(k, top + 1))
   - All accessed elements are valid by I₂

3. I₃ preservation:
   - Stack size and top remain unchanged
   - Therefore, relationship remains valid

### 4. Edge Case Analysis

#### 4.1 Empty Stack

- Push: Correctly moves from -1 to 0
- Pop: Returns -1, maintains empty state
- Increment: No effect (limit = min(k, 0) = 0)

#### 4.2 Full Stack

- Push: Operation skipped, maintains full state
- Pop: Correctly removes top element
- Increment: Operates on all elements normally

#### 4.3 Boundary Cases

1. Single Element:

   ```
   - Push when empty: top: -1 → 0
   - Pop when single: top: 0 → -1
   ```

2. Maximum Size:
   ```
   - Push at max: Operation skipped
   - Increment with k > stack size: Correctly limited
   ```

### 5. Final Theorem

**Theorem**: The CustomStack implementation maintains invariants I₁, I₂, and I₃ through all operations and correctly implements the stack interface with increment functionality.

**Proof**: By mathematical induction:

1. Base case: Proven in initialization
2. Inductive step: Proven for each operation
3. Edge cases: Explicitly verified

Therefore, the implementation is correct for all possible sequences of operations within the given constraints.

> **Technical Note**: The implementation prioritizes simplicity and robustness over potential optimizations. For example, the increment operation could be optimized for large k values using lazy propagation, but this would complicate the implementation and might not be worth it given the constraint that val ≤ 100.
