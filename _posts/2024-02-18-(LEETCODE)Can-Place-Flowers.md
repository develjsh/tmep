---
layout: post
title:  "(LEETCODE)605. Can Place Flowers"
summary: "Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return true if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule and false otherwise."
author: seunghwan
date: '2024-02-18 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Array
usemathjax: false
permalink: /blog/leetcode/Can-Place-Flowers/
---
## 605. Can Place Flowers

### Situation

You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array `flowerbed` containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer `n`, return `true` *if* `n` *new flowers can be planted in the* `flowerbed` *without violating the no-adjacent-flowers rule and* `false` *otherwise*.

**Constraints:**

- `1 <= flowerbed.length <= 2 * 104`
- `flowerbed[i]` is `0` or `1`.
- There are no two adjacent flowers in `flowerbed`.
- `0 <= n <= flowerbed.length`

### Task

- for 문을 통해 0이면 양옆에 값을 구해 0인지 확인합니다.
    - 양옆이 0 이면 중간 값을 0으로 바꾸고 cnt 객체 값을 올려 몇개를 심을 수 있는지 파악합니다.
- 만약 인덱스 0 이 0이고 인덱스 1 이 0이면 심을 수 있습니다.
    - (i == 0 or flowerbed[i-1] == 0)
    - (i == len(flowerbed)-1 or flowerbed[i+1] == 0)

### Action

```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        if n == 0:
            return True
            
        cnt = 0
        for i in range(len(flowerbed)):
            if flowerbed[i] == 0 and (i == 0 or flowerbed[i-1] == 0) and (i == len(flowerbed)-1 or flowerbed[i+1] == 0):
                flowerbed[i] = 1
                cnt += 1
        
        return n <= cnt
```

### Result

Runtime: 74 ms
Memory: 17.90 MB