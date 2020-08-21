---
title: Minimum Possible Integer After At Most K Adjacent Swaps On Digits's Solution
date: 2020-07-05 12:03:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Minimum Possible Integer After At Most K Adjacent Swaps On Digits](https://leetcode.com/problems/minimum-possible-integer-after-at-most-k-adjacent-swaps-on-digits)
**Original Solution Post**: [Solution](https://leetcode.com/problems/minimum-possible-integer-after-at-most-k-adjacent-swaps-on-digits/discuss/720127/python-bytedance-interview-question)

**Idea**<br>
Greedily select the smallest number we can reach, and push it all the way to the front.




**Complexity**




- time: O(n^2)
- space: O(n)



**Python**




```
class Solution:
    def minInteger(self, num: str, k: int) -> str:
        num = [*map(int, num)]
        if k >= (len(num) ** 2) // 2:
            return ''.join(map(str, sorted(num)))
        
        res = []
        q = [(v, i) for i, v in enumerate(num)]
        while k and q:
            idx, (v, i) = min(enumerate(q[:k + 1]), key=lambda p:p[1])
            k -= idx
            del q[idx]
            res += v,
            
        res += [v for v, _ in q]
        return ''.join(map(str, res))

```



Please please don't downvote unless necessary bro.
