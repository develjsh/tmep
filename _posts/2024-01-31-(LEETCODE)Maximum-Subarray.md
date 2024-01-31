---
layout: post
title:  "(LEETCODE)Maximum Subarray"
summary: "Given an integer array nums, find the subarray with the largest sum, and return its sum."
author: seunghwan
date: '2024-01-31 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Arrays
usemathjax: false
permalink: /blog/adding-categories-tags-in-posts/
---
## Maximum Subarray


## Situation

Given an integer array nums, find the subarray with the largest sum, and return *its sum.*

## Task

연속적으로 숫자를 합 했을 때 최대 값을 구하라.

## Action

동적 계획법(Dynamic Programming)을 사용하여 최적 부분 구조를 갖고 있습니다. 배열 **`dp`**를 사용하여 각 위치에서의 최대 부분 배열 합을 계산하고, 이전 위치에서 계산한 값들을 활용하여 현재 위치의 값을 계산합니다. 이런 방식으로 한 번의 순회로 최대 부분 배열 합을 찾을 수 있습니다. 따라서 전체 시간 복잡도는 O(n)입니다.

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [num for num in nums]
        for i in range(1, len(nums)):
            dp[i] = max(dp[i-1]+nums[i], nums[i])
        return max(dp)
```

# Result

    Runtime: 524 ms; Beats 92.26%

    Memory: 32.02 MB; Beats 6.80%