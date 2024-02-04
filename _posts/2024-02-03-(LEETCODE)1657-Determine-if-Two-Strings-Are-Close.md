---
layout: post
title:  "(LEETCODE)1657. Determine if Two Strings Are Close"
summary: "Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target."
author: seunghwan
date: '2024-02-03 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Arrays
usemathjax: false
permalink: /blog/leetcode/1657-Determine-if-Two-Strings-Are-Close/
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
    - 문제의 핵심은 word1 과 word2 의

1. ChatGPT 정답.
    
    
2. 다른 사람 코드.
    
    1. **`Counter`**를 사용하여 각각의 **`word1`**과 **`word2`**에서 문자 빈도를 계산하는 과정은 O(n) 시간이 걸립니다. 여기서 n은 입력 문자열의 길이입니다.
    2. 각각의 단어에서 문자 빈도 값을 정렬하는 과정은 O(k log k) 시간이 걸립니다. 여기서 k는 각 단어에서 고유 문자의 수입니다. 최악의 경우에는 k를 상수로 간주할 수 있으므로 정렬은 O(1) 시간 복잡도를 갖습니다.
    3. 문자의 집합(고유 문자들의 집합)이 일치하는지 확인하는 과정도 O(k) 시간이 걸립니다. 여기서 k는 고유 문자의 수입니다.
    
    지배적인 요소를 고려하면, 즉 정렬하는 과정이 가장 높은 시간 복잡도를 갖기 때문에 전체적인 시간 복잡도는 O(n log n)입니다.
    

### Result

    Runtime: 52 ms; Beats 96.54%

    Memory: 18.67 MB; Beats 9.71%