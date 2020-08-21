---
title: Closest Divisors's Solution
date: 2020-02-23 12:43:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Closest Divisors](https://leetcode.com/problems/closest-divisors)
**Original Solution Post**: [Solution](https://leetcode.com/problems/closest-divisors/discuss/517722/Python-4-line-O(logn))

**Complexity**<br>
Time: `O(logn)`<br>
Space: `O(1)`




**Python 3**




```
class Solution:
    def closestDivisors(self, num: int) -> List[int]:
        for k in range(int(math.sqrt(num+2)), 0, -1):
            for x in (num+1, num+2):
                if not x % k:
                    return x // k, k

```


