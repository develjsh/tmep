---
layout: post
title:  "(LEETCODE)104. Maximum Depth of Binary Tree"
summary: "Given the `root` of a binary tree, return *its maximum depth. A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node."
author: seunghwan
date: '2024-02-07 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Depth First Search
usemathjax: false
permalink: /blog/leetcode/Maximum-Depth-of-Binary-Tree/
---
## 104. Maximum Depth of Binary Tree

### Situation

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `100 <= Node.val <= 100`

### Task

### Action

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root is None:
            return 0
        else:
            left_depth = self.maxDepth(root.left)
            right_depth = self.maxDepth(root.right)
            return max(left_depth, right_depth) + 1
```
### Result

Runtime: 35 ms; Beats 93.11%

Memory: 17.60 MB; Beats 97.96%

