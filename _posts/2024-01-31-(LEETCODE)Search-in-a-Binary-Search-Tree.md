---
layout: post
title:  "(LEETCODE)700. Search in a Binary Search Tree"
summary: "You are given the root of a binary search tree (BST) and an integer val. Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null."
author: seunghwan
date: '2024-02-08 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Depth First Search
usemathjax: false
permalink: /blog/leetcode/Search-in-a-Binary-Search-Tree/
---
## 700. Search-in-a-Binary-Search-Tree

### Situation

You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 5000]`.
- `1 <= Node.val <= 107`
- `root` is a binary search tree.
- `1 <= val <= 107`

### Task

### Action

1. 새로 함수를 만들어서 Node.val 값을 비교합니다. 비교 후 맞으면 Node 를 return 합니다.
    
    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
            def bst(node):
                if node is None:
                    return None
                    
                if node.val == val:
                    return node
                
                left_result = bst(node.left)
                if left_result:
                    return left_result
    
                right_result = bst(node.right)
                if right_result:
                    return right_result
    
            return bst(root)
    ```
    

# Result

1. 좋은 결과지만 문제 점이 있었습니다.

**Trouble

1. **`bst`** 함수는 명시적으로 어떤 값을 반환하지 않는 재귀 함수입니다. 파이썬에서는 함수에 반환 문이 없으면 암시적으로 **`None`**을 반환합니다. 따라서 **`searchBST`** 메서드 내에서 **`bst(root)`**를 호출하면 항상 **`None`**을 반환합니다.
2. **`bst`** 함수 내의 print 문은 디버깅에 유용하지만 실제로 노드를 찾았을 때 결과를 반환하려면 함수를 수정해야 합니다.

결과적으로 bst() 함수를 호출하고 나서는 return 값을 넣어줘야 했습니다.
