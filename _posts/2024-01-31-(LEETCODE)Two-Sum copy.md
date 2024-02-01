---
layout: post
title:  "(LEETCODE)Two sum"
summary: "Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target."
author: seunghwan
date: '2024-01-31 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Arrays
usemathjax: false
permalink: /blog/adding-categories-tags-in-posts/
---
## Convert Sorted Array to Binary Search Tree

### Situation

Given an integer array nums where the elements are sorted in ascending order, convert *it to a height-balanced binary search tree*.

### Task

BST 에 대해서 찾아보고 height-balanced 도 찾아 봤지만 어떻게 이게 가능한건지 모르겠습니다.

### Action

유튜브 보고 코드를 작성했습니다.

이 코드의 시간 복잡도는 O(nlogn) 입니다. Binary Search Tree 에서 가장 좋은 시간 복잡도 입니다. 가장 안 좋은 것은 0(n) 입니다.

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
            def bst(l, r):
                if l > r:
                    return None
                
                m = (l + r) // 2
                root = TreeNode(nums[m])
                root.left = bst(l, m - 1)
                root.right = bst(m + 1, r)
                return root
            
            return bst(0, len(nums) - 1)
    ```

### Result

Runtime: 52 ms; Beats 96.54%

Memory: 18.67 MB; Beats 9.71%