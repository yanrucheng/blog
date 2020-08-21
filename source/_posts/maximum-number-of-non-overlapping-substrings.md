---
title: Maximum Number Of Non Overlapping Substrings's Solution
date: 2020-07-19 12:18:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Maximum Number Of Non Overlapping Substrings](https://leetcode.com/problems/maximum-number-of-non-overlapping-substrings)
**Original Solution Post**: [Solution](https://leetcode.com/problems/maximum-number-of-non-overlapping-substrings/discuss/743166/O(n)-set-solution)

**Idea**<br>
The idea is that we compute a "feature value" for s[0..i] for every i within [0..n-1], we then stores them in a set for quick query.




**Complexity**<br>
Time: O(n)<br>
Space: O(n)




**Python**




```
class Solution:
    def maxNumOfSubstrings(self, s: str) -> List[str]:
        cnt = collections.Counter(s)
        feat = [0] * 26
        seen = {(0,) * 26: 0}
        order = []
        full = [cnt[c] for c in string.ascii_lowercase]
        ans = []
        
        for i, c in enumerate(s):
            if c not in order: order.append(c)
            feat[ord(c) - ord('a')] += 1                
            tmp = feat[:]
            for x in order[::-1]:
                if tmp[ord(x) - ord('a')] < full[ord(x) - ord('a')]:
                    break
                tmp[ord(x) - ord('a')] = 0
                if tuple(tmp) in seen:
                    ans.append(s[seen[tuple(tmp)]: i + 1])
                    seen = {(0) * 26: 0}
                    order = []
            seen[tuple(feat)] = i + 1
        return ans

```


