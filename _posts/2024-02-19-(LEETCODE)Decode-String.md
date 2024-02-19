---
layout: post
title:  "(LEETCODE)394. Decode String"
summary: "Given an encoded string, return its decoded string."
author: seunghwan
date: '2024-02-19 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Stack
usemathjax: false
permalink: /blog/leetcode/Decode-String/
---
## 338. Counting Bits

### Situation

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

### Task

### Action

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack = []

        for i in range(len(s)):
            if s[i] != "]":
                stack.append(s[i])
            else:
                substr = ""
                while stack[-1] != "[":
                    substr = stack.pop() + substr
                stack.pop()

                k = ""
                while stack and stack[-1].isdigit():
                    k = stack.pop() + k
                stack.append(int(k) * substr)
        
        return "".join(stack)
```

### Result

Runtime: 35 ms

Memory: 16.59 MB