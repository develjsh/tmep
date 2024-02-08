---
layout: post
title:  "(LEETCODE)2352. Equal Row and Column Pairs"
summary: "Given a 0-indexed n x n integer matrix grid, return the number of pairs (ri, cj) such that row ri and column cj are equal."
author: seunghwan
date: '2024-02-06 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Arrays
usemathjax: false
permalink: /blog/leetcode/Equal-Row-and-Column-Pairs/
---
## 2352. Equal Row and Column Pairs

### Situation

Given a **0-indexed** `n x n` integer matrix `grid`, *return the number of pairs* `(ri, cj)` *such that row* `ri` *and column* `cj` *are equal*.

A row and column pair is considered equal if they contain the same elements in the same order (i.e., an equal array).

### Task

파라미터로 받은 grid 의 행과 열을 비교해서 같은 것이 몇개 있는지 return 하면 됩니다.

### Action

1. 이중 for문.
    - gird 값을 for문으로 돌려서 row 값을 가져오기.
        - TypeError: None is not valid value for the expected return type integer
            → return 0 를 안해줘서 발생한 error 였습니다.
    
```python
    class Solution:
    def equalPairs(self, grid: List[List[int]]) -> int:
        count = 0
        for idx in range(len(grid)):
            ylist = []
            for ydx in range(len(grid)):
                ylist.append(grid[ydx][idx])
            
            for row in grid:
                if row == ylist:
                    count += 1

        return count
```

### Result

Runtime: 520 ms

Memory: 21.31 MB