---
layout: post
title:  "(LEETCODE)338. Counting Bits"
summary: "Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet."
author: seunghwan
date: '2024-02-17 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Stack
usemathjax: false
permalink: /blog/leetcode/Counting-Bits/
---
## 338. Counting Bits

### Situation

Given an integer `n`, return *an array* `ans` *of length* `n + 1` *such that for each* `i` **(`0 <= i <= n`)*,* `ans[i]` *is the **number of*** `1`***'s** in the binary representation of* `i`.

### Task

- 단순하게 이진코드 값을 구하는 코드를 구현합니다.
    - 구현하면서 값을 stack 에 넣습니다.
    - while 문이 종료되면 count() 함수로 1의 값을 구한 후 result stack 에 넣습니다.

### Action

```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        result = []
        for i in range(n+1):
            stack = []
            while i:
                n1 = i%2
                stack.append(n1)
                i = i//2
            result.append(stack.count(1))

        return result
```

### Result

Runtime: 123 ms

Memory: 223.21 MB