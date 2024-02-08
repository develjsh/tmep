---
layout: post
title:  "(LEETCODE)1448. Count Good Nodes in Binary Tree"
summary: "Given a binary tree `root`, a node *X* in the tree is named **good** if in the path from root to *X* there are no nodes with a value *greater than* X. Return the number of **good** nodes in the binary tree."
author: seunghwan
date: '2024-02-09 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Depth-First Search
usemathjax: false
permalink: /blog/leetcode/Count-Good-Nodes-in-Binary-Tree/
---
## 1448. Count Good Nodes in Binary Tree

### Situation
Given a binary tree `root`, a node *X* in the tree is named **good** if in the path from root to *X* there are no nodes with a value *greater than* X.

Return the number of **good** nodes in the binary tree.

**Constraints:**

- The number of nodes in the binary tree is in the range `[1, 10^5]`.
- Each node's value is between `[-10^4, 10^4]`.

### Task

1. node 가 1개라도 있으면 return 값이 1 이기 때문에 return 값 count 가 1이 되게 해줍니다.
2. 만약 node 값이 없으면 0 을 return 하게 해줍니다. 동시에 재귀함수에서 root 값이 없으면 0 을 return 해줍니다,
3. 재귀함수 호출하고 나서 결과를 count 를 return 해줘야 합니다.

### Action

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        def countFunction(root, v):
            if root is None:
                return 0

            count = 0
            if root.val >= v:
                count += 1

            v = max(root.val, v)

            count += countFunction(root.left, v)
            count += countFunction(root.right, v)

            return count

        return countFunction(root, root.val)
```

### Result

Runtime: 130 ms

Memory: 35.51 MB