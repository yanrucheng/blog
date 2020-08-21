---
title: Maximum Sum Bst In Binary Tree's Solution
date: 2020-03-08 00:07:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Maximum Sum Bst In Binary Tree](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree)
**Original Solution Post**: [Solution](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/discuss/531800/python-easy-traversal-with-explanation)

**Idea**<br>
For each subtree, we return 4 elements.




1. the status of this subtree, `1` means it's empty, `2` means it's a BST, `0` means it's not a BST
1. size of this subtree (we only care about size of BST though)
1. the minimal value in this subtree
1. the maximal value in this subtree



Then we only need to make sure for every BST




- both of its children are BST
- the right bound of its left child is smaller than `root.val`
- the left bound of its right child is larger than `root.val`



**Complexity**<br>
Time: `O(N)`<br>
Space: `O(logN)` for function calls, worst case `O(N)` if the given tree is not balanced




**Python 3**




```
class Solution:
    def maxSumBST(self, root: TreeNode) -> int:
        res = 0
        def traverse(root):
            '''return status_of_bst, size_of_bst, left_bound, right_bound'''
            nonlocal res
            if not root: return 1, 0, None, None # this subtree is empty
            
            ls, l, ll, lr = traverse(root.left)
            rs, r, rl, rr = traverse(root.right)
            
            if ((ls == 2 and lr < root.val) or ls == 1) and ((rs == 2 and rl > root.val) or rs == 1):
		        # this subtree is a BST
                size = root.val + l + r
                res = max(res, size)
                return 2, size, (ll if ll is not None else root.val), (rr if rr is not None else root.val)
            return 0, None, None, None # this subtree is not a BST
        
        traverse(root)
        return res

```


