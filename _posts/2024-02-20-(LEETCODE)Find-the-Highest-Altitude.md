---
layout: post
title:  "(LEETCODE)1732. Find the Highest Altitude"
summary: "You are given an integer array gain of length n where gain[i] is the net gain in altitude between points i and i + 1 for all (0 <= i < n). Return the highest altitude of a point."
author: seunghwan
date: '2024-02-20 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Stack
usemathjax: false
permalink: /blog/leetcode/Find-the-Highest-Altitude/
---
## 1732. Find the Highest Altitude

### Situation

There is a biker going on a road trip. The road trip consists of `n + 1` points at different altitudes. The biker starts his trip on point `0` with altitude equal `0`.

You are given an integer array `gain` of length `n` where `gain[i]` is the **net gain in altitude** between points `i` and `i + 1` for all (`0 <= i < n)`. Return *the **highest altitude** of a point.*

### Task

- max() 함수를 사용하여 계속해서 최대 고도 값을 가지고 있으면 최종적으로 return 해줍니다.

### Action

```python
class Solution:
    def largestAltitude(self, gain: List[int]) -> int:
        ans = h = 0
        for v in gain:
            h += v
            ans = max(ans, h)
        return ans
```

### Result

Runtime: 42 ms
Memory: 16.46 MB