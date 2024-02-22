---
layout: post
title:  "(LEETCODE)724. Find Pivot Index"
summary: "Return the leftmost pivot index. If no such index exists, return -1."
author: seunghwan
date: '2024-02-13 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Array
usemathjax: false
permalink: /blog/leetcode/Find-Pivot-Index/
---
## 724. Find Pivot Index

### Situation

Given an array of integers `nums`, calculate the **pivot index** of this array.

The **pivot index** is the index where the sum of all the numbers **strictly** to the left of the index is equal to the sum of all the numbers **strictly** to the index's right.

If the index is on the left edge of the array, then the left sum is `0` because there are no elements to the left. This also applies to the right edge of the array.

Return *the **leftmost pivot index***. If no such index exists, return `-1`.

### Task

- 왼쪽과 오른쪽을 구분해서 for문으로 sum의 값을 비교합니다.

### Action

```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        for i in range(len(nums)):
            if sum(nums[:i]) == sum(nums[i + 1:]):
                return i
        
        return -1
```

### Result

Runtime: 6396 ms
Memory: 17.81 MB