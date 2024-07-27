+++
title = 'Problem 284 Peeking Iterator'
date = 2024-07-27T18:37:52+05:30
draft = false
series = 'leetcode'
tags =['class','iterator','design']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 284](https://leetcode.com/problems/peeking-iterator/description/)

## Question

Design an `iterator` that supports the `peek` operation on an `existing iterator` in addition to the `hasNext` and the `next` operations.

Implement the PeekingIterator class:

- `PeekingIterator(Iterator<int> nums)` Initializes the object with the given integer iterator `iterator`.
- `int next()` Returns the next element in the array and moves the pointer to the next element.
- `boolean hasNext()` Returns true if there are still elements in the array.
- `int peek()` Returns the next element in the array without moving the pointer.

Note: Each language may have a different implementation of the constructor and Iterator, but they all support the `int next()` and `boolean hasNext()` functions.

## Example 1

```
Input
["PeekingIterator", "next", "peek", "next", "next", "hasNext"]
[[[1, 2, 3]], [], [], [], [], []]
Output
[null, 1, 2, 2, 3, false]
Explanation
PeekingIterator peekingIterator = new PeekingIterator([1, 2, 3]); // [1,2,3]
peekingIterator.next();    // return 1, the pointer moves to the next element [1,2,3].
peekingIterator.peek();    // return 2, the pointer does not move [1,2,3].
peekingIterator.next();    // return 2, the pointer moves to the next element [1,2,3]
peekingIterator.next();    // return 3, the pointer moves to the next element [1,2,3]
peekingIterator.hasNext(); // return False
```

## Constraints

```markdown
- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- All the calls to `next` and `peek` are valid.
- At most 1000 calls will be made to `next`, `hasNext`, and `peek`.
```

## Solution

```cpp
/*
 * Below is the interface for Iterator, which is already defined for you.
 * **DO NOT** modify the interface for Iterator.
 *
 *  class Iterator {
 *		struct Data;
 * 		Data* data;
 *  public:
 *		Iterator(const vector<int>& nums);
 * 		Iterator(const Iterator& iter);
 *
 * 		// Returns the next element in the iteration.
 *		int next();
 *
 *		// Returns true if the iteration has more elements.
 *		bool hasNext() const;
 *	};
 */

class PeekingIterator : public Iterator {
public:
    int currNext=-1;
    bool currHasNext=false;
	PeekingIterator(const vector<int>& nums) : Iterator(nums) {
	    // Initialize any member here.
	    // **DO NOT** save a copy of nums and manipulate it directly.
	    // You should only use the Iterator interface methods.
        currHasNext = Iterator::hasNext();
        if(currHasNext){
            currNext = Iterator::next();
        }

	}

    // Returns the next element in the iteration without advancing the iterator.
	int peek() {
        return currNext;
	}

	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	int next() {
        int result = currNext;
        currHasNext = Iterator::hasNext();
        if(currHasNext){
            currNext = Iterator::next();
        }
        return result;

	}

	bool hasNext() const {
        return currHasNext;
	}
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Peek      | O(1)            | O(1)             |
```

## Explanation

### 1. Intuition

```markdown
- the given class `iterator` has the following functions:
  - `int next()`
  - `boolean hasNext()`
- `int next()` will return the next element and increments the iterator.
- `boolean hasNext()` will return a boolean value if there is an element present next

- We need to implement a `peek` function that will return the next element without incrementing the iterator.
- Hence we need to cache the next element and the boolean value if there is an element present next.
- We will use the `currNext` and `currHasNext` variables to store the next element and the boolean value if there is an element present next.
- We will initialize the `currNext` and `currHasNext` variables in the constructor.
- In the constructor, we will check if there is an element present next and store the next element in the `currNext` variable.
- In the `peek` function, we will return the `currNext` variable.
- In the `next` function, we will return the `currNext` variable and update the `currNext` and `currHasNext` variables.
- In the `hasNext` function, we will return the `currHasNext` variable.
```

### 2. Implementation

```markdown
- We will create a class `PeekingIterator` that will inherit the `Iterator` class.
- We will create two variables `currNext` and `currHasNext` to store the next element and the boolean value if there is an element present next.
- In the constructor, we will initialize the `currNext` and `currHasNext` variables.
- In the `peek` function, we will return the `currNext` variable.
- In the `next` function
  - We will store the `currNext` variable in the `result` variable.
  - if there is an element present next, we will update the `currNext` and `currHasNext` variables.
  - return `result`
- In the `hasNext` function return the `currHasNext` variable.
```
