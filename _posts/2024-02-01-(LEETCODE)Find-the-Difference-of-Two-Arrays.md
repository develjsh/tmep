---
layout: post
title:  "(LEETCODE)Find the Difference of Two Arrays"
summary: "Given two 0-indexed integer arrays nums1 and nums2, return a list answer of size 2."
author: seunghwan
date: '2024-02-02 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Hash Table
usemathjax: false
permalink: /blog/leetcode/find the differenceof two arrays/
---

## Situation

Given two **0-indexed** integer arrays `nums1` and `nums2`, return *a list* `answer` *of size* `2` *where:*

- `answer[0]` *is a list of all **distinct** integers in* `nums1` *which are **not** present in* `nums2`*.*
- `answer[1]` *is a list of all **distinct** integers in* `nums2` *which are **not** present in* `nums1`.

**Note** that the integers in the lists may be returned in **any** order.

## Task

- 시간 복잡도를 생각해서 해결해야 합니다.

## Action

각 nums1 과 nums2 를 for 문으로 돌리면 시간 복잡도가 O(n)으로 나올꺼고 코드 작성도 어렵지 않을꺼라고 생각했습니다. 그리고 마지막으로 set() 을 통해 중복된 값을 제거했습니다.

```python
class Solution:
    def findDifference(self, nums1: List[int], nums2: List[int]) -> List[List[int]]:
        t1 = []
        t2 = []

        for i in nums1:
            if i not in nums2:
                t1.append(i)
        
        for i in nums2:
            if i not in nums1:
                t2.append(i)
        
        return [set(t1), set(t2)]
```

## Result

Runtime: 557 ms; Beats 17.16%

Memory: 17.62 MB; Beats 44.79%
