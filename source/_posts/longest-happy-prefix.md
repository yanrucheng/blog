---
title: Longest Happy Prefix's Solution
date: 2020-03-22 13:13:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Longest Happy Prefix](https://leetcode.com/problems/longest-happy-prefix)
**Original Solution Post**: [Solution](https://leetcode.com/problems/longest-happy-prefix/discuss/547192/Python-O(n)-time-KMP-Search-with-explanation)

**Idea**<br>
This is a typical string-matching problem. More details [here](http://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm).<br>
All we need to do is to find the first occurrence of pattern `s[0...n-2]` in `s[1...n-1]` in O(n) time.




**Complexity**<br>
Time: `O(n)`<br>
Space: `O(n)`




**Python 3**




```
class Solution:
    def longestPrefix(self, s: str) -> str:
        if len(s) == 1: return ''
        
        def KMPSearch(pat, txt): 
            M, N = len(pat), len(txt) 
            lps = failure(pat)

            i = 0 # index for txt[] 
            j = 0 # index for pat[] 
            while i < N: 
                if pat[j] == txt[i]: 
                    i += 1
                    j += 1
                elif i < N and pat[j] != txt[i]: 
                    if j != 0: 
                        j = lps[j-1] 
                    else: 
                        i += 1
                if i >= N:                        
                    return i - j 

        def failure(pat): 
            res = [0]
            i, target = 1, 0
            while i < len(pat): 
                if pat[i]== pat[target]: 
                    target += 1
                    res += target,
                    i += 1
                elif target: 
                    target = res[target-1] 
                else: 
                    res += 0,
                    i += 1
            return res                        
                        
        l = len(s) - 1 - KMPSearch(s[:-1], s[1:])
        return s[:l]

```


