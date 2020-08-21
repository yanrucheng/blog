---
title: Find A Value Of A Mysterious Function Closest To Target's Solution
date: 2020-07-25 10:47:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Find A Value Of A Mysterious Function Closest To Target](https://leetcode.com/problems/find-a-value-of-a-mysterious-function-closest-to-target)
**Original Solution Post**: [Solution](https://leetcode.com/problems/find-a-value-of-a-mysterious-function-closest-to-target/discuss/743142/on2-simple-brute-force-solution)

**Idea**<br>
The idea is that & operation will only makes a number smaller. For most cases, `arr[i]` loses at least 1 bit, and let `arr[i]` drop to zero in O(log(arr[i])) steps.




**Complexity**<br>
Time: O(n^2), but perform very much like O(nlogK) for the above reasons, where K = max(arr).<br>
Space: O(1)




Please leave an upvote if it helps, they mean a lot to me.




**Python**




```
class Solution:
    def closestToTarget(self, arr: List[int], target: int) -> int:
        res = math.inf
        
        for i, x in enumerate(arr):
            curr = x
            diff = abs(target - curr)
            for j in range(i + 1, len(arr)):
                x_ = arr[j]
                curr &= x_
                if abs(curr - target) >= diff:                    
                    break
                diff = abs(curr - target)
            res = min(res, diff)
                
        return res
                

```


