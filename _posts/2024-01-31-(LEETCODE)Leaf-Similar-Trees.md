---
layout: post
title:  "(LEETCODE)872. Leaf-Similar Trees"
summary: "Return true if and only if the two given trees with head nodes root1 and root2 are leaf-similar."
author: seunghwan
date: '2024-02-08 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Depth First Search
usemathjax: false
permalink: /blog/leetcode/Search-in-a-Binary-Search-Tree/
---
## 872. Leaf-Similar Trees

### Situation

Consider all the leaves of a binary tree, from left to right order, the values of those leaves form a **leaf value sequence***.*

For example, in the given tree above, the leaf value sequence is `(6, 7, 4, 9, 8)`.

Two binary trees are considered *leaf-similar* if their leaf value sequence is the same.

Return `true` if and only if the two given trees with head nodes `root1` and `root2` are leaf-similar.

**Constraints:**

- The number of nodes in each tree will be in the range `[1, 200]`.
- Both of the given trees will have values in the range `[0, 200]`.

### Task

### Action

1.ChatGPT

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:
        def leafSimilar(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:
            def get_leaf_values(root, leaves):
                if root:
                    if not root.left and not root.right:
                        leaves.append(root.val)
                    get_leaf_values(root.left, leaves)
                    get_leaf_values(root.right, leaves)

            leaves1, leaves2 = [], []
            get_leaf_values(root1, leaves1)
            get_leaf_values(root2, leaves2)

            return leaves1 == leaves2
    ```

# Result

1. The time complexity of the **`leafSimilar`** function is O(N + M), where N is the number of nodes in the first tree (root1) and M is the number of nodes in the second tree (root2). This is because the function performs a depth-first traversal of both trees, visiting each node once.