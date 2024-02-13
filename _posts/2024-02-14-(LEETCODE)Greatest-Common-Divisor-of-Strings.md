---
layout: post
title:  "(LEETCODE)1071. Greatest Common Divisor of Strings
summary: "Given two strings str1 and str2, return the largest string x such that x divides both str1 and str2."
author: seunghwan
date: '2024-02-14 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: String
usemathjax: false
permalink: /blog/leetcode/Greatest-Common-Divisor-of-Stringsy/
---
## 1071. Greatest Common Divisor of Strings

### Situation

For two strings `s` and `t`, we say "`t` divides `s`" if and only if `s = t + ... + t` (i.e., `t` is concatenated with itself one or more times).

Given two strings `str1` and `str2`, return *the largest string* `x` *such that* `x` *divides both* `str1` *and* `str2`.

### Task

- **`str1 + str2 == str2 + str1`**이 성립하는 경우에만 두 문자열의 최대 공약수가 존재합니다.
- 주어진 문제에서 두 문자열 **`str1`**과 **`str2`**의 최대 공약수를 구하는 것은 두 문자열이 공통으로 가지고 있는 가장 긴 부분 문자열을 찾는 것과 동일합니다.
- 두 문자열 **`str1`**과 **`str2`**의 최대 공약수를 구하는 방법은 다음과 같습니다:
    1. 먼저, 두 문자열의 길이를 비교하여 더 짧은 문자열의 길이를 구합니다.
    2. 해당 길이까지 반복하여, 두 문자열이 같은 부분 문자열을 가지고 있는지 확인합니다.
    3. 만약 두 문자열이 동일한 부분 문자열을 가지고 있다면, 그 부분 문자열이 두 문자열의 최대 공약수가 됩니다.

### Action

```python
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        if str1 + str2 != str2 + str1:
            return ""
        
        def gcd(a, b):
            while b:
                a, b = b, a % b
            return a
            
        return str1[:gcd(len(str1), len(str2))]
```

### Result

Runtime: 37 ms

Memory: 16.61 MB