+++
title = 'Problem 874 Walking Robot Simulation'
date = 2024-09-24T18:43:34+05:30
draft = false
series = 'leetcode'
tags =['math','simulation','array']
toc = false
math = false
+++

# Problem Statement

**Link** - [Problem 874](https://leetcode.com/problems/walking-robot-simulation/description/)

## Question

A robot on an infinite XY-plane starts at point `(0, 0)` facing north. The robot receives an array of integers `commands`, which represents a sequence of moves that it needs to execute. There are only three possible types of instructions the robot can receive:

- `-2`: Turn left 90 degrees.
- `-1`: Turn right 90 degrees.
- `1` <= k <= `9`: Move forward `k` units, one unit at a time.

Some of the grid squares are obstacles. The `i`<sup>th</sup> obstacle is at grid point `obstacles[i] = (xi, yi)`. If the robot runs into an obstacle, it will stay in its current location (on the block adjacent to the obstacle) and move onto the next command.

Return the maximum squared Euclidean distance that the robot reaches at any point in its path (i.e. if the distance is 5, return 25).

Note:

There can be an obstacle at `(0, 0)`. If this happens, the robot will ignore the obstacle until it has moved off the origin. However, it will be unable to return to `(0, 0)` due to the obstacle.

- North means +Y direction.
- East means +X direction.
- South means -Y direction.
- West means -X direction.

## Example 1

```
Input: commands = [4,-1,3], obstacles = []

Output: 25

Explanation:

The robot starts at (0, 0):

Move north 4 units to (0, 4).
Turn right.
Move east 3 units to (3, 4).
The furthest point the robot ever gets from the origin is (3, 4), which squared is 3^2 + 4^2 = 25 units away.
```

## Example 2

```
nput: commands = [4,-1,4,-2,4], obstacles = [[2,4]]

Output: 65

Explanation:

The robot starts at (0, 0):

Move north 4 units to (0, 4).
Turn right.
Move east 1 unit and get blocked by the obstacle at (2, 4), robot is at (1, 4).
Turn left.
Move north 4 units to (1, 8).
The furthest point the robot ever gets from the origin is (1, 8), which squared is 1^2 + 8^2 = 65 units away.
```

## Example 3

```
Input: commands = [6,-1,-1,6], obstacles = [[0,0]]

Output: 36

Explanation:

The robot starts at (0, 0):

Move north 6 units to (0, 6).
Turn right.
Turn right.
Move south 5 units and get blocked by the obstacle at (0,0), robot is at (0, 1).
The furthest point the robot ever gets from the origin is (0, 6), which squared is 6^2 = 36 units away.
```

## Constraints

- 1 <= `commands.length` <= 10<sup>4</sup>
- `commands[i]` is either -2, -1, or an integer in the range [1, 9].
- 0 <= `obstacles.length` <= 10<sup>4</sup>
- -3 x 10<sup>4</sup> <= `x`<sub>`i`</sub>, `y`<sub>`i`</sub><= 3 x 10<sup>4</sup>
- The answer is guaranteed to be less than 2<sup>31</sup>.

## Solution

```cpp
class Solution {
public:
    int robotSim(vector<int>& commands, vector<vector<int>>& obstacles) {
        ios::sync_with_stdio(false);
        cin.tie(nullptr);

        int i=0,j=0,res = 0,direction=0;
        set<pair<int,int>> obsSet;
        for(const auto &it : obstacles){
            obsSet.insert({it[0],it[1]});
        }

        for(const auto & command : commands){
            if(command == -1){
                direction = (direction+1) % 4;
            }
            else if(command == -2){
                direction = (direction+3)%4;
            }

            else{
                int dist = command;
                while(dist > 0){
                    if(direction == 0){
                        if(obsSet.find({i,j+1}) != obsSet.end())
                            break;
                        j++;
                    }
                    else if(direction == 1){
                        if(obsSet.find({i+1,j}) != obsSet.end())
                            break;
                        i++;
                    }
                    else if(direction == 2){
                        if(obsSet.find({i,j-1}) != obsSet.end())
                            break;
                        j--;
                    }
                    else{
                        if(obsSet.find({i-1,j}) != obsSet.end())
                            break;
                        i--;
                    }
                    res = max(res,i*i + j*j);
                    dist--;
                }
            }
        }
        return res;
    }
};
```

## Complexity Analysis

```markdown
| Algorithm | Time Complexity | Space Complexity |
| --------- | --------------- | ---------------- |
| Hashset   | O(N)            | O(N)             |
| Traversal | O(N)            | O(1)             |
```

## Explanation

### 1. Intuition

- There is no special technique to solve this problem, we just need to follow the instructions.
- Consider 4 directions starting from north in clockwise order as 0,1,2,3.
- Robots starts from `(0,0)` facing north. We need to keep track of `i` and `j` coordinates and `direction`.
- Create a hashset of obstacles to check if the next move is an obstacle or not in O(1) time.
- Traverse the commands array and follow the instructions.
- If the command is `-1` then turn right by incrementing the direction by 1 and taking modulo 4.
- If the command is `-2` then turn left by decrementing the direction by 3 and taking modulo 4.
- If the command is number from 1 to 9 then
  - First check if the next move is an obstacle or not.
  - If not then move in the direction of the robot and update the coordinates.
  - If its an obstacle then break the loop.
  - Update the result by taking the maximum of the current result and the squared euclidean distance.
  - Decrement the distance by 1.
- Return the result after traversing all the commands.

### 2. Implementation

```markdown
- Intialize the variables `i`, `j`, `res` and `direction` to 0.
- Create a hashset `obsSet` to store the obstacles.
- Traverse the obstacles array and insert the obstacles into the hashset.
- Traverse the commands array and follow the instructions.
- If the command is `-1` then turn right by incrementing the direction by 1 and taking modulo 4.
- If the command is `-2` then turn left by decrementing the direction by 3 and taking modulo 4.
- If the command is number from 1 to 9 then
  - First check if the next move is an obstacle or not.
  - If not then move in the direction of the robot and update the coordinates according to the direction.
    - If the direction is 0 then increment the `j` coordinate.
    - If the direction is 1 then increment the `i` coordinate.
    - If the direction is 2 then decrement the `j` coordinate.
    - If the direction is 3 then decrement the `i` coordinate.
  - If its an obstacle then break the loop.
  - Update the result by taking the maximum of the current result and the squared euclidean distance.
  - Decrement the distance by 1.
- Return the result.
```
