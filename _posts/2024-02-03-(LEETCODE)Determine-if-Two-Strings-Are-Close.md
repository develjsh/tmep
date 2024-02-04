---
layout: post
title:  "(LEETCODE)1657. Determine if Two Strings Are Close"
summary: "Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target."
author: seunghwan
date: '2024-02-03 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Arrays
usemathjax: false
permalink: /blog/leetcode/Determine-if-Two-Strings-Are-Close/
---
## 1657. Determine if Two Strings Are Close
### Situation

Two strings are considered **close** if you can attain one from the other using the following operations:

- Operation 1: Swap any two **existing** characters.
    - For example, `abcde -> aecdb`
- Operation 2: Transform **every** occurrence of one **existing** character into another **existing** character, and do the same with the other character.
    - For example, `aacabb -> bbcbaa` (all `a`'s turn into `b`'s, and all `b`'s turn into `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings, `word1` and `word2`, return `true` *if* `word1` *and* `word2` *are **close**, and* `false` *otherwise.*

### Task

- 문제 이해 및 암기하기.

### Action

- 문제 이해하기.
    
```python

class Solution:
    def closeStrings(self, word1: str, word2: str) -> bool:
        # 두 문자열의 길이가 다르면 연산 불가능
        if len(word1) != len(word2):
            return False

        # 두 문자열의 문자 빈도를 계산
        counter1 = Counter(word1)
        counter2 = Counter(word2)

        # 두 문자열의 문자 빈도가 같은지 확인
        if sorted(counter1.values()) != sorted(counter2.values()):
            return False

        # 두 문자열의 문자 종류가 같은지 확인
        if set(word1) != set(word2):
            return False

        return True
```
        
```python

class Solution:
    def closeStrings(self, word1: str, word2: str) -> bool:
        frequency_word1 = Counter(word1)
        frequency_word2 = Counter(word2)
                #Counter({'a': 1, 'b': 1, 'c': 1})

        sorted_values_word1 = sorted(frequency_word1.values())
        sorted_values_word2 = sorted(frequency_word2.values())
        
        keys_match = set(frequency_word1.keys()) == set(frequency_word2.keys())

        return sorted_values_word1 == sorted_values_word2 and keys_match
```

1. **`Counter`**를 사용하여 각각의 **`word1`**과 **`word2`**에서 문자 빈도를 계산하는 과정은 O(n) 시간이 걸립니다. 여기서 n은 입력 문자열의 길이입니다.
2. 각각의 단어에서 문자 빈도 값을 정렬하는 과정은 O(k log k) 시간이 걸립니다. 여기서 k는 각 단어에서 고유 문자의 수입니다. 최악의 경우에는 k를 상수로 간주할 수 있으므로 정렬은 O(1) 시간 복잡도를 갖습니다.
3. 문자의 집합(고유 문자들의 집합)이 일치하는지 확인하는 과정도 O(k) 시간이 걸립니다. 여기서 k는 고유 문자의 수입니다.

지배적인 요소를 고려하면, 즉 정렬하는 과정이 가장 높은 시간 복잡도를 갖기 때문에 전체적인 시간 복잡도는 O(n log n)입니다.
    

### Result

Runtime: 52 ms; Beats 96.54%

Memory: 18.67 MB; Beats 9.71%