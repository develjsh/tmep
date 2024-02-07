---
layout: post
title:  "(LEETCODE)1926. Nearest Exit from Entrance in Maze"
summary: "Return the number of steps in the shortest path from the entrance to the nearest exit, or -1 if no such path exists."
author: seunghwan
date: '2024-02-07 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Bread-First Search
usemathjax: false
permalink: /blog/leetcode/Nearest-Exit-from-Entrance-in-Maze/
---
## 1926. Nearest Exit from Entrance in Maze

### Situation

You are given an `m x n` matrix `maze` (**0-indexed**) with empty cells (represented as `'.'`) and walls (represented as `'+'`). You are also given the `entrance` of the maze, where `entrance = [entrancerow, entrancecol]` denotes the row and column of the cell you are initially standing at.

In one step, you can move one cell **up**, **down**, **left**, or **right**. You cannot step into a cell with a wall, and you cannot step outside the maze. Your goal is to find the **nearest exit** from the `entrance`. An **exit** is defined as an **empty cell** that is at the **border** of the `maze`. The `entrance` **does not count** as an exit.

Return *the **number of steps** in the shortest path from the* `entrance` *to the nearest exit, or* `-1` *if no such path exists*.

**Example 1:**

```
Input: maze = [["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]
Output: 1
Explanation: There are 3 exits in this maze at [1,0], [0,2], and [2,3].
Initially, you are at the entrance cell [1,2].
- You can reach [1,0] by moving 2 steps left.
- You can reach [0,2] by moving 1 step up.
It is impossible to reach [2,3] from the entrance.
Thus, the nearest exit is [0,2], which is 1 step away.

```

**Example 2:**

```
Input: maze = [["+","+","+"],[".",".","."],["+","+","+"]], entrance = [1,0]
Output: 2
Explanation: There is 1 exit in this maze at [1,2].
[1,0] does not count as an exit since it is the entrance cell.
Initially, you are at the entrance cell [1,0].
- You can reach [1,2] by moving 2 steps right.
Thus, the nearest exit is [1,2], which is 2 steps away.

```

**Example 3:**

```
Input: maze = [[".","+"]], entrance = [0,0]
Output: -1
Explanation: There are no exits in this maze.

```

**Constraints:**

- `maze.length == m`
- `maze[i].length == n`
- `1 <= m, n <= 100`
- `maze[i][j]` is either `'.'` or `'+'`.
- `entrance.length == 2`
- `0 <= entrancerow < m`
- `0 <= entrancecol < n`
- `entrance` will always be an empty cell.

### Task

### Action

1. 다른 사람 코드.
    
```python
rows, cols = len(maze), len(maze[0])
#the 4 directions we are allowed to move: up, down, left, right
directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

#the entrance does not count as an exit
#we might as well mark it as a wall/visited
start_row, start_col = entrance
maze[start_row][start_col] = '+'

#we start out bfs from the entrance
#which means we initialize our queue with coordinates of the entrance
#and 0 for the current distance from the starting point
q = deque([(start_row, start_col, 0)])

while q:
    curr_row, curr_col, curr_distance = q.popleft()
    #for the current node in queue, lets check the 4 possible neighbors
    for d in directions:
        next_row = curr_row + d[0]
        next_col = curr_col + d[1]
        #if the neighbor is unvisited, its empty and if its within bounds
        #we can check if its unvisted just by checking if its a "." - because we turn it into a "+" when we visited it
        if 0 <= next_row < rows and 0 <= next_col < cols and maze[next_row][next_col] == ".":
            #if this empty cell is an exit - check if its at the borders of the matrix
            #return curr distance + 1
            if 0 == next_row or next_row == rows - 1 or 0 == next_col or next_col == cols-1:
                return curr_distance + 1
            #otherwise we havent found the exit yet
            #which means we want to explore the next level
            #lets just add the node to the q, increment the distance
            #and mark it as visited -> turn it into a wall
            maze[next_row][next_col] = '+'
            q.append((next_row, next_col, curr_distance + 1))

#if we exit the while loop without returning
#means that we could not find an exit - we just return -1
return -1
```
    

### Result

1. 다른 사람 코드.
    - 시간 복잡도: O(rows * cols)