---
title: Construct Target Array With Multiple Sums's Solution
date: 2020-02-18 12:33:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Construct Target Array With Multiple Sums](https://leetcode.com/problems/construct-target-array-with-multiple-sums)
**Original Solution Post**: [Solution](https://leetcode.com/problems/construct-target-array-with-multiple-sums/discuss/510338/python-replace-reversely-pass-1e911-priority-queue)

The Idea is to reversely construct `[1, 1, ..., 1]` from `target`.




**Complexity**<br>
Given the steps `K` to construct the target:<br>
Time: O(KlogK)<br>
Space: O(N)




**Updated Python 3**




```
import heapq
class Solution:
    def isPossible(self, A):
        q = [-x for x in A]
        heapq.heapify(q)
        sm = sum(A)
        while True:
            high, rest = -q[0], sm + q[0]
            if high == 1 or rest == 1: return True
            original = high % rest
            if rest >= high or not original: return False
            sm -= high - original
            heapq.heapreplace(q, -original)

```



**Orignal Python 3**




```
import heapq
class Solution:
    def isPossible(self, target: List[int]) -> bool:
        sm = sum(target)
        q = [-x for x in target]
        heapq.heapify(q)
        while True:
            high = -q[0]
			original = 2*high - sm
            if original > high or original < 1: return False
            sm -= high - original
            heapq.heapreplace(q, -original)
            if all(x == -1 for x in q):
                return True

```


