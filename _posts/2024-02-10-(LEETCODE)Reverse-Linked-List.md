---
layout: post
title:  "(LEETCODE)206. Reverse Linked List"
summary: "Given the head of a singly linked list, reverse the list, and return the reversed list."
author: seunghwan
date: '2024-02-10 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Linked List
usemathjax: false
permalink: /blog/leetcode/Reverse-Linked-List/
---
## 206. Reverse Linked List

### Situation

Given the `head` of a singly linked list, reverse the list, and return the reversed list.

### Task
- ListNode 가 무엇인가?
    
    단순하게 생성자를 통해 value를 주입하고 다음 노드를 세팅하는 방식으로 사용하거나 생성자에서 다음 노드를 함께 받아 처리할 수 있는 구조입니다.
    
    하지만 예시에서는 ListNode를 표현할 때 [0 -> 2 -> 1 -> 4 -> 5]이런식으로 head Node에서 시작해 연결된 노드들을 화살표 등으로 표현하거나 단순히 배열 처럼 [0, 2, 1, 4, 5] 이런식으로 표현하기도 합니다.
    
- for문
    
    ListNode 는 for 문을 돌 수 없습니다.

### Action

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Initialize prev pointer as NULL...
        prev = None
        # Initialize the curr pointer as the head...
        curr = head
        # Run a loop till curr points to NULL...
        while curr:
            # Initialize next pointer as the next pointer of curr...
            next = curr.next
            # Now assign the prev pointer to curr’s next pointer.
            curr.next = prev
            # Assign curr to prev, next to curr...
            prev = curr
            curr = next
        return prev       # Return the prev pointer to get the reverse linked list...
```


### Result

Runtime: 40 ms

Memory: 17.71 MB