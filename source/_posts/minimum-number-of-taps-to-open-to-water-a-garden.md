---
title: Minimum Number Of Taps To Open To Water A Garden's Solution
date: 2020-01-19 14:08:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Minimum Number Of Taps To Open To Water A Garden](https://leetcode.com/problems/minimum-number-of-taps-to-open-to-water-a-garden)
**Original Solution Post**: [Solution](https://leetcode.com/problems/minimum-number-of-taps-to-open-to-water-a-garden/discuss/484272/python-o1-memory-on-time-greedy-with-explanation-132ms-beats-100)

**Constant Space! O(n) Greedy Approuch (AC: 132ms beats 100%)**<br>
This question is very similar to [Jump Game II](https://leetcode.com/problems/jump-game-ii/).




1. Inplace modify `ranges` so that each element represents the furthest index you can reach by taking one jump.
1. Find the minimal jump you need to reach the end.

```
class Solution:
    def minTaps(self, n: int, ranges: List[int]) -> int:
        for i,r in enumerate(ranges):
            l = max(0,i-r)
            ranges[l] = max(i+r, ranges[l])
            
        res = lo = hi = 0            
        while hi < n:
            lo, hi = hi, max(ranges[lo:hi+1])
            if hi == lo: return -1
            res += 1
        return res  

```



**O(nlogn) Greedy Approuch (AC: 175ms)**




1. Convert `ranges` in the format of `[(l0,r0), (l1,r1)...]`, where `li,ri` represents the left boundary and right boundary of a tap. O(n)
1. Sort the taps. O(nlogn)
1. Select taps greedily. We select the one that can reach the furthest among those whose left boundary overlaps with the current furthest point.

```
class Solution:
    def minTaps(self, n: int, ranges: List[int]) -> int:
        taps = sorted([(max(p-r,0), min(p+r,n)) for p, r in enumerate(ranges)])
        
        new_furthest = furthest = ind = 0
        for res in range(1,n+1):
            for i,(l,r) in enumerate(taps[ind:],ind):
                if l > furthest: break
                new_furthest = max(new_furthest, r)
            ind = i                
                
            if new_furthest == furthest and new_furthest != n: return -1
            if new_furthest == n: return res
            furthest = new_furthest

```


