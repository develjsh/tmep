---
layout: post
title:  "(LEETCODE)735. Asteroid Collision"
summary: "Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet."
author: seunghwan
date: '2024-02-16 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Stack
usemathjax: false
permalink: /blog/leetcode/Asteroid-Collision/
---
## 735. Asteroid Collision

### Situation

We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

### Task

### Action

```python
class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        stack = []
        while asteroids:
            print("stack = ", stack)
            v1 = asteroids.pop()
            if len(stack) < 1:
                stack.append(v1)
                continue
            v2 = stack.pop()
            print("v1 = ", v1)
            if (v1 < 0 and v2 < 0) or (v1 > 0 and v2 > 0):
                stack.append(v2)
                stack.append(v1)
                continue
            if v1 < 0:
                stack.append(v2)
                stack.append(v1)
                continue
            elif abs(v1) > abs(v2):
                stack.append(v1)
            elif abs(v1) < abs(v2):
                stack.append(v2)

        return list(reversed(stack))
```

245번 testcase 에서 Wron Answer 가 나왔습니다.

input 은 [-2,2,-1,-2] 이고 정답은 [-2] 입니다.

문제는 asteroids 비교를 뒤에서 부터 했던 것이 문제였습니다. 앞에서부터 처리하도록 로직을 수정하면 해결됩니다.

**[정답 코드]**

```python
class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        stack = []
        for asteroid in asteroids:
            while stack and asteroid < 0 < stack[-1]:
                if stack[-1] < -asteroid:
                    stack.pop()
                    continue
                elif stack[-1] == -asteroid:
                    stack.pop()
                break
            else:
                stack.append(asteroid)
        return stack
```

### Result

Runtime: 74 ms

Memory: 17.90 MB