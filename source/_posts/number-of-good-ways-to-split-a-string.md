---
title: Number Of Good Ways To Split A String's Solution
date: 2020-07-26 00:14:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Number Of Good Ways To Split A String](https://leetcode.com/problems/number-of-good-ways-to-split-a-string)
**Original Solution Post**: [Solution](https://leetcode.com/problems/number-of-good-ways-to-split-a-string/discuss/754660/C%2B%2BPython-8-line-constant-space-simple-solution)

Please upvote if you find this post helpful or interesting. It means a lot to me. Thx~




**Complexity**




- Time: `O(n)`
- Space: `O(26) = O(1)`



**Python**




```
class Solution:
    def numSplits(self, s: str) -> int:
        p, total = set(), collections.Counter(s)
        res, total_cnt = 0, len(total)
        for i, x in enumerate(s[::-1]):
            p.add(x)
            total[x] -= 1
            total_cnt -= not total[x]
            res += len(p) == total_cnt
        return res    

```



**C++**




```
class Solution {
public:
    int numSplits(string s) {
        map<char, int> total;
        for (auto c: s) total[c]++;
        int total_size = (int)total.size(), res = 0;
        set<char> p;
        for (auto c: s) {
            p.insert(c);
            total[c]--;
            total_size -= !total[c];
            res += (total_size == p.size());
        }
        return res;
    }
};

```


