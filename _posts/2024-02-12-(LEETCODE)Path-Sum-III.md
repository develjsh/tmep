---
layout: post
title:  "(LEETCODE)437. Path Sum III"
summary: "Given the root of a binary tree and an integer targetSum, return the number of paths where the sum of the values along the path equals targetSum."
author: seunghwan
date: '2024-02-12 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Depth-First Search
usemathjax: false
permalink: /blog/leetcode/Path-Sum-III/
---
## 437. Path Sum III

### Situation
Given the `root` of a binary tree and an integer `targetSum`, return *the number of paths where the sum of the values along the path equals* `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

### Task

### Action

- 실패
```python
User
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        def getCnt(root, s):
            count = 0
            if root is None:
                return 0

            if root.val + s == targetSum:
                count += 1
            count += getCnt(root.left, s + root.val)
            count += getCnt(root.right, s + root.val)
            count += getCnt(root.left, 0)
            count += getCnt(root.right, 0)
            
            return count
        return getCnt(root, 0)
```
    

- 성공
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        # prefix sums encountered in current path
        sums = defaultdict(int)
        sums[0] = 1

        def dfs(root, total):
            count = 0
            if root:
                total += root.val
                # Can remove sums[currSum-targetSum] prefixSums to get target
                count = sums[total-targetSum]

                # Add value of this prefixSum
                sums[total] += 1
                # Explore children
                count += dfs(root.left, total) + dfs(root.right, total)
                # Remove value of this prefixSum (path's been explored)
                sums[total] -= 1

            return count

        return dfs(root, 0)
```
### Result

Runtime: 48 ms

Memory: 17.00 MB