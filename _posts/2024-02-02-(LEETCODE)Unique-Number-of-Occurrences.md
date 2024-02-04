---
layout: post
title:  "(LEETCODE)1207. Unique Number of Occurrences"
summary: "Given two strings, word1 and word2, return true if word1 and word2 are close, and false otherwise."
author: seunghwan
date: '2024-02-02 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Hash Table
usemathjax: false
permalink: /blog/leetcode/Unique-Number-of-Occurrences/
---
## 1207. Unique Number of Occurrences

### Situation

Given an array of integers `arr`, return `true` *if the number of occurrences of each value in the array is **unique** or* `false` *otherwise*.

### Task

Given an array of integers `arr`, return `true` *if the number of occurrences of each value in the array is **unique** or* `false` *otherwise*.

### Action

1. 처음에 [] 배열에 max(arr) + 값에 크기에 0 을 넣고 나중에 len() 로 비교할려고 했지만 값에 음수 값이 여러개 들어 있으면 실패합니다.
    
```python
class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        l = [0 for _ in range(max(arr) + 1)]
        for n in arr:
            l[n] += 1
        
        l = [i for i in l if i != 0]
        if len(set(l)) == len(l):
            return True

        return False
```
    
2. 중첩 for문을 사용하지 않고 최대한 시간 복잡도를 O(n)으로 유지할 수 있도록 코드를 작성했습니다.
    
```python
class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        nd = {}
        for i in arr:
            nd[i] = 0
        
        for i in arr:
            nd[i] += 1
        
        l = []
        for _, v in enumerate(nd):
            l.append(nd[v])
        
        if len(set(l)) == len(l):
            return True

        return False
```
    

### Result
Runtime: 137 ms; Beats 50.004%

Memory: 17.60 MB; Beats 99.71%