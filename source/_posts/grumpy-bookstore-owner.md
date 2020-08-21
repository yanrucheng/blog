---
title: Grumpy Bookstore Owner's Solution
date: 2019-05-29 11:00:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Grumpy Bookstore Owner](https://leetcode.com/problems/grumpy-bookstore-owner)
**Original Solution Post**: [Solution](https://leetcode.com/problems/grumpy-bookstore-owner/discuss/300164/python-beat-99-5-line-intuitive-solution-constant-space)

A more readable version:




```
class Solution:
    def maxSatisfied(self, customers, grumpy, X):
        effect, maxe, maxi = 0, 0, -1
		
        for i, g in enumerate(grumpy[X:]):
			if g == 1:
				effect += customers[i+X]
			if grumpy[i] == 1:
				effect -= customers[i]
			if maxe < effect:
				maxe = effect
				maxi = i
				
        return sum(x for i,x in enumerate(customers) if grumpy[i] == 0 or (i > maxi and i <= maxi+X))    

```



The five-line version:




```
class Solution:
    def maxSatisfied(self, customers: List[int], grumpy: List[int], X: int) -> int:
        effect, maxe, maxi = 0, 0, -1
        for i,g in enumerate(grumpy[X:]):
            effect += (customers[i+X] if g==1 else 0) + (-customers[i] if grumpy[i]==1 else 0)
            if maxe < effect: maxe, maxi = effect, i
        return sum(x for i,x in enumerate(customers) if grumpy[i] == 0 or (i > maxi and i <= maxi+X))                

```


