---
title: Remove All Adjacent Duplicates In String's Solution
date: 2019-05-20 10:25:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string)
**Original Solution Post**: [Solution](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/discuss/295660/python-on-linked-list-solution)

Notes:




- Explicitly Breaking the object reference circle in the doubly linked list improves the memory usage by around 4 MB.

```
class ListNode:
    __slots__ = 'val next prev'.split()
    def __init__(self, x):
        self.val = x
        self.next = None
        self.prev = None

class Solution:
    def removeDuplicates(self, S: str) -> str:
        self.root = ll = self.list_to_ll(S)
        while True:
            if not ll or not ll.next or not ll.next.next: break
            if ll.next.val == ll.next.next.val:
                ll.next.next.prev = None # break the hanging nodes, should have used a weak ref
                ll.next = ll.next.next.next
                if ll.next:
                    ll.next.prev = ll
                ll = ll.prev or ll
            else: 
                ll = ll.next
        return self.ll_to_s(self.root)
    
    def ll_to_s(self, ll):
        res = ''
        while ll:
            res += ll.val
            ll = ll.next
        return res            
        
    def list_to_ll(self,l):
        cur = dummy = ListNode('')
        for x in l:
            last = cur
            cur.next = cur = ListNode(x)
            cur.prev = last
        return dummy

```


