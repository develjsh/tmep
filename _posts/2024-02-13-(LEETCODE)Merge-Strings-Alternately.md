---
layout: post
title:  "(LEETCODE)1768. Merge Strings Alternately
summary: "You are given two strings word1 and word2. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string."
author: seunghwan
date: '2024-02-13 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Two Pointers
usemathjax: false
permalink: /blog/leetcode/Merge-Strings-Alternately/
---
## 1768. Merge Strings Alternately

### Situation

You are given two strings `word1` and `word2`. Merge the strings by adding letters in alternating order, starting with `word1`. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return *the merged string.*

### Task

- word1, 결과에 첫 글자를 가지고 있는, 을 먼저 for 문을 돌면서 word2 길이를 넘지 않게 같이 호출하면서 string 값에 += 해줍니다.
- 만약 word2 가 더 길면 l1 길이로 시작해서 word2 에 나머지 값을 string 값에 합쳐줍니다.

### Action

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        result = ""
        l1 = len(word1)
        l2 = len(word2)
        for i in range(len(word1)):
            result += word1[i]
            if i < len(word2):
                result += word2[i]
        if l1 < l2:
            result += word2[len(word1):]
        return result
```

### Result

Runtime: 28 ms

Memory: 16.56 MB