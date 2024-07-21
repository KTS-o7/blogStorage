+++
title = 'Problem 752 Open the Lock'
date = 2024-04-22T16:27:12+05:30
draft = false
series = 'leetcode'
tags = ['cpp','bfs','graph']
toc = false
+++

# Problem Statement

**Link** - [Problem 752](https://leetcode.com/problems/open-the-lock/description/)

## Question

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'.` The wheels can rotate freely and wrap around: for example we can turn `'9'` to be `'0'`, or `'0'` to be `'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at `'0000'`, a string representing the state of the 4 wheels.

You are given a list of `deadends` dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a `target` representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

### Note

- What they want us to find is the shortest path from the start node to the target node.
- The start node is `'0000'` and the target node is the `target` string.
- There will be an edge between two nodes if they differ by one digit.
- There are no outgoing edges from the deadend nodes.
- We can reach the target by changing the digits of the start node one by one, We can change them in either direction.
- The logic what we will be using is of `BFS`. BFS will always give us the shortest path.
- **Note that the starting node also maybe a deadend.**

## Example 1

```text
Input: deadends = ["0201","0101","0102","1212","2002"],
target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
```

## Example 2

```text
Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation: We can turn the last wheel in reverse to move from "0000" -> "0009".
```

## Example 3

```text
Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"],
target = "8888"
Output: -1
Explanation: We cannot reach the target without getting stuck.
```

## Constraints

- `1` <= deadends.length <= `500`
- `deadends[i].length == 4`
- `target.length` == `4`
- `target` will **not** be in the list `deadends`.
- `target` and `deadends[i]` **consist of digits only**.

## Solution

```cpp
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        std::ios::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        unordered_set<string> dead(deadends.begin(),deadends.end());

        if(dead.find("0000") != dead.end())
            return -1;
        if(target == "0000")
            return 0;

        int steps = 0;
        queue<string> q;
        q.push("0000");
        char actual;
        while(!q.empty())
        {
            steps++;
            for(int size = q.size();size>0;size--)
            {
                string passwd = q.front();
                q.pop();
                for(int i= 0;i<4;i++)
                {
                    actual = passwd[i];
                    passwd[i] = (passwd[i]=='9')?'0':passwd[i]+1;

                    if(passwd == target)
                        return steps;
                    if(dead.find(passwd) == dead.end())
                    {
                        q.push(passwd);
                        dead.insert(passwd);
                    }
                    passwd[i] = actual;

                    passwd[i] = (passwd[i]=='0')?'9':passwd[i]-1;

                    if(passwd == target)
                        return steps;
                    if(dead.find(passwd) == dead.end())
                    {
                        q.push(passwd);
                        dead.insert(passwd);
                    }
                    passwd[i] = actual;
                }

            }
        }
        return -1;
    }
};
```

## Complexity Analysis

- `Time complexity` - O(10000) = O(1) - As we are iterating over all the possible combinations of the lock.
- `Space complexity` - O(10000) = O(1) - As we are storing all the possible combinations of the lock in the queue.

## Explanation

1. First we create a `unordered_set` of `deadends` which will help us to check if the current combination is a deadend or not.
2. We add all the deadends to the set.
3. This makes sure that immidiate children of the deadends are not added to the queue.
4. We check if the `target` is already a deadend or not.
5. We check if the `target` is the start node itself.
6. We create a queue and push the start node to it.
7. We will keep track on number steps needed to reach the `target`.
8. We will iterate over the **_current queue size_**. This is because we need to check only the current level nodes.
9. We will iterate over the `current node` and change the digits one by one.
10. If the child node is the `target` we return the number of steps.
11. If child node is not `target` and is not a deadend or not visited till now, we will add it to the queue and mark it as visited.
12. We will do the same for the other direction of the digit.
13. If we are not able to reach the `target` we return -1.

---

> **Note** - This is a very good problem to understand the concept of BFS. We can also solve this problem using DFS but BFS is more efficient in this case.
