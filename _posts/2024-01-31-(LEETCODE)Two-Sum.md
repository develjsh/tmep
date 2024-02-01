---
layout: post
title:  "(LEETCODE)Two sum"
summary: "Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target."
author: seunghwan
date: '2024-01-31 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Arrays
usemathjax: false
permalink: /blog/leetcode/two-sum/
---
## Two sum

### Situation

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

### Task

두 개의 값이 합이 target 이 되었을 때 그 값들의 index 를 배열로 넘기세요.

### Action

이중 for 문 을 사용했습니다.

브루트 포스(Brute Force) 방식 사용했습니다. 브루트 포스는 가능한 모든 조합을 시도하여 답을 찾는 방법으로, 간단하지만 효율성이 낮을 수 있습니다.

시간 복잡도를 계산해 보면, 이 코드의 중첩된 반복문이 사용되고 있습니다. 외부 반복문은 배열 nums의 각 요소에 대해 순회하고, 내부 반복문은 해당 요소 이후의 모든 요소와의 합을 계산합니다. 따라서 이 코드의 시간 복잡도는 O(n^2)입니다. 여기서 n은 배열 nums의 길이입니다.

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i, num in enumerate(nums):
            for j, num2 in enumerate(nums[i + 1:len(nums)]):
                if (num + num2) == target:
                    return [i, i + j + 1]
```

### Result

Runtime: 453 ms; Beats 48.86%

Memory: 14.10 MB; Beats 90.74%

### 더 나은 방법

1. 해쉬 맵
    
    해시 맵을 사용하여 한 번의 순회로 문제를 해결합니다.
    
    한 번의 순회로 해시 맵을 구축하면서 동시에 타겟에서 현재 숫자를 뺀 값이 해시 맵에 있는지 확인합니다. 만약 존재한다면 해당 값의 인덱스와 현재 인덱스를 반환하여 문제를 해결합니다. 이 방법의 시간 복잡도는 O(n)이므로 효율적입니다.
    
    ```python
    class Solution:
        def twoSum(self, nums: List[int], target: int) -> List[int]:
            num_dict = {}  # 해시 맵을 저장할 딕셔너리
    
                complement = target - num  # 현재 숫자와 더해져서 target이 되어야 하는 수
    
            for i, num in enumerate(nums): 
                # 딕셔너리에서 더해져서 target이 되어야 하는 수가 있는지 확인
                if complement in num_dict:
                    return [num_dict[complement], i]
                
                # 딕셔너리에 현재 숫자와 인덱스를 저장
    
                num_dict[num] = i
            # 만약 문제에서 보장된 답이 있다면 여기까지 도달하지 않음
            return []  # 보장된 답이 없을 경우 빈 리스트 반환
    ```

    Runtime: 52 ms; Beats 96.54%

    Memory: 18.67 MB; Beats 9.71%