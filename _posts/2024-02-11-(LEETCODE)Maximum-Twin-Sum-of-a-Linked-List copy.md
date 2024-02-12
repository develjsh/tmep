---
layout: post
title:  "(LEETCODE)2130. Maximum Twin Sum of a Linked List"
summary: "Given the head of a linked list with even length, return the maximum twin sum of the linked list."
author: seunghwan
date: '2024-02-11 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Linked List
usemathjax: false
permalink: /blog/leetcode/Maximum-Twin-Sum-of-a-Linked-List/
---
## 2130. Maximum Twin Sum of a Linked List

### Situation

In a linked list of size `n`, where `n` is **even**, the `ith` node (**0-indexed**) of the linked list is known as the **twin** of the `(n-1-i)th` node, if `0 <= i <= (n / 2) - 1`.

- For example, if `n = 4`, then node `0` is the twin of node `3`, and node `1` is the twin of node `2`. These are the only nodes with twins for `n = 4`.

The **twin sum** is defined as the sum of a node and its twin.

Given the `head` of a linked list with even length, return *the **maximum twin sum** of the linked list*.

### Task

- 재귀 함수를 통해 ListNode 를 list 형태로 전환합니다.
- for 문을 통해 각 node 의 twin 값을 구합니다.
- result 값과 sum 값을 비교 후 sum 값이 크면 result 를 sum 으로 바꿉니다.

### Action

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def pairSum(self, head: Optional[ListNode]) -> int:
        def getList(l, head):
            if head is None:
                return l
            l.append(head.val)
            return getList(l, head.next)
        
        l = []
        getList(l, head)
        
        result = 0
        size = len(l)
        for idx, v in enumerate(l):
            tNum = (size - 1 - idx)
            if 0 >= tNum and tNum >= ((size / 2) - 1):
                continue
            sum = l[idx] + l[tNum]
            if result < sum:
                result = sum

        return result
```

### Result

시간 복잡도는 O(n^2)입니다.

Runtime: 381 ms
Memory: 52.88 MB

### Better way

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def pairSum(self, head: Optional[ListNode]) -> int:
        slow, fast = head, head
        prev = None

        while fast and fast.next:
            fast = fast.next.next
            tmp = slow.next
            slow.next = prev
            prev = slow
            slow = tmp
        
        res = 0
        while slow:
            res = max(res, prev.val + slow.val)
            prev = prev.next
            slow = slow.next
        return res
```

시간 복잡도는 O(n) 입니다.