---
layout: post
title:  "(LEETCODE)2390. Removing Stars From a String"
summary: "Return the string after all stars have been removed."
author: seunghwan
date: '2024-02-15 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Stack
usemathjax: false
permalink: /blog/leetcode/Removing-Stars-From-a-String/
---
## 2390. Removing Stars From a String

### Situation

You are given a string `s`, which contains stars `*`.

In one operation, you can:

- Choose a star in `s`.
- Remove the closest **non-star** character to its **left**, as well as remove the star itself.

Return *the string after **all** stars have been removed*.

**Note:**

- The input will be generated such that the operation is always possible.
- It can be shown that the resulting string will always be unique.

### Task

- stack = [] 객체 생성.
- str 값을 list 형태로 변환.
- for 문 돌면서 * 값 찾기
    - * 찾으면 .pop() 을 통해 그 전에 넣은 값 제거.
    - 못찾으면 stack 객체에 넣기
- stack 에 담긴 값들을 문자열로 만든후 return.

### Action

```python
class Solution:
    def removeStars(self, s: str) -> str:
        stack = []
        sList = list(s)
        for v in sList:
            if v == "*":
                stack.pop()
            else:
                stack.append(v)

        return "".join(stack)
```

### Result

Runtime: 132 ms

Memory: 18.66 MB