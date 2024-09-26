+++
title = 'Problem 729 My Calendar I'
date = 2024-09-26T22:13:02+05:30
draft = false
series = 'leetcode'
tags =['array','sorting','binray-search','hash-map']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 729](https://leetcode.com/problems/my-calendar-i/description/)

## Question

You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a double booking.

A double booking happens when two events have some non-empty intersection (i.e., some moment is common to both events.).

The event can be represented as a pair of integers `start` and `end` that represents a booking on the half-open interval `[start, end)`, the range of real numbers `x` such that start `<= x < end`.

Implement the `MyCalendar` class:

- `MyCalendar()` Initializes the calendar object.
- `boolean book(int start, int end)` Returns `true` if the event can be added to the calendar successfully without causing a double booking. Otherwise, return `false` and do not add the event to the calendar.

## Example 1

```
Input
["MyCalendar", "book", "book", "book"]
[[], [10, 20], [15, 25], [20, 30]]
Output
[null, true, false, true]

Explanation
MyCalendar myCalendar = new MyCalendar();
myCalendar.book(10, 20); // return True
myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event.
myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.
```

## Constraints

- 0 <= `start < end` <= 10<sup>9</sup>
- At most `1000` calls will be made to `book`.

## Solution

```cpp
class MyCalendar {
private:
    vector<pair<int, int>> calendar;

public:
    bool book(int start, int end) {
        for (const auto [s, e] : calendar) {
            if (start < e && s < end) {
                return false;
            }
        }
        calendar.emplace_back(start, end);
        return true;
    }
};

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar* obj = new MyCalendar();
 * bool param_1 = obj->book(start,end);
 */
```

## Complexity Analysis

```markdown
| Algorithm                                           | Time Complexity | Space Complexity |
| --------------------------------------------------- | --------------- | ---------------- |
| Process every input and then add it to the calendar | O(N^2)          | O(N)             |
```

## Explanation

### 1. Intuition

- Based on the constraints, We can do a brute force approach and still get an accepted solution.
- If we store the events in a vector of pairs, we can check if the new event begins before the end of the previous event and ends after the start of the previous event.
- An event is not overlapping if `s1` is greater than `e2` or `e1` is less than `s2`. Basically any event should not start before the end of the previous event and should not end after the start of the previous event.

### 2. Implementation

```markdown
- Initialize a vector of pairs to store the events.
- For every new event in calendar,
  - `start` and `end` are the start and end of the new event.
  - If `start < e` and `s < end`, return false.
  - Otherwise, add the new event to the calendar and return true.
```

> This is basic implementation of Object Oriented Programming in C++.
> There is also a better solution using `map` and `Binary Search` which is more optimized and efficient.

```cpp
class MyCalendar {
private:
    set<pair<int, int>> calendar;

public:
    bool book(int start, int end) {
        const pair<int, int> event{start, end};
        const auto nextEvent = calendar.lower_bound(event);
        if (nextEvent != calendar.end() && nextEvent->first < end) {
            return false;
        }

        if (nextEvent != calendar.begin()) {
            const auto prevEvent = prev(nextEvent);
            if (prevEvent->second > start) {
                return false;
            }
        }

        calendar.insert(event);
        return true;
    }
};
```
