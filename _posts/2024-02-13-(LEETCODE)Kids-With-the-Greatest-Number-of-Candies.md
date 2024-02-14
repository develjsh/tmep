---
layout: post
title:  "(LEETCODE)1431. Kids With the Greatest Number of Candies"
summary: "Return a boolean array result of length n, where result[i] is true if, after giving the ith kid all the extraCandies, they will have the greatest number of candies among all the kids, or false otherwise."
author: seunghwan
date: '2024-02-15 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Arrays
usemathjax: false
permalink: /blog/leetcode/Kids-With-the-Greatest-Number-of-Candies/
---
## 1431. Kids With the Greatest Number of Candies

### Situation

here are `n` kids with candies. You are given an integer array `candies`, where each `candies[i]` represents the number of candies the `ith` kid has, and an integer `extraCandies`, denoting the number of extra candies that you have.

Return *a boolean array* `result` *of length* `n`*, where* `result[i]` *is* `true` *if, after giving the* `ith` *kid all the* `extraCandies`*, they will have the **greatest** number of candies among all the kids, or* `false` *otherwise*.

Note that **multiple** kids can have the **greatest** number of candies.

### Task

- word1, 결과에 첫 글자를 가지고 있는, 을 먼저 for 문을 돌면서 word2 길이를 넘지 않게 같이 호출하면서 string 값에 += 해줍니다.
- 만약 word2 가 더 길면 l1 길이로 시작해서 word2 에 나머지 값을 string 값에 합쳐줍니다.

### Action

```python
class Solution:
    def kidsWithCandies(self, candies: List[int], extraCandies: int) -> List[bool]:
        result = [True for i in range(len(candies))]
        a = [i + extraCandies for i in candies]
        m = max(candies)
        for idx, n in enumerate(a):
            if n < m:
                result[idx] = False

        return result
```

### Result

Runtime: 43 ms

Memory: 16.54 MB