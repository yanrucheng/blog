---
title: The K Th Lexicographical String Of All Happy Strings Of Length N's Solution
date: 2020-04-19 00:01:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [The K Th Lexicographical String Of All Happy Strings Of Length N](https://leetcode.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n)
**Original Solution Post**: [Solution](https://leetcode.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n/discuss/585561/Python-7-line-DFS-with-yield)

**Idea**<br>
The property of DFS helps us iterate through all the possibilities in ascending order automatically. All we need to do is to output the kth result.




**Python**




```
class Solution:
    def getHappyString(self, n: int, k: int) -> str:
        def generate(prev):
            if len(prev) == n:
                yield prev; return
            yield from (res for c in 'abc' if not prev or c != prev[-1] for res in generate(prev + c))
            
        for i, res in enumerate(generate(''), 1):            
            if i == k: return res
        return ''     

```



**Python, more readable**




```
class Solution:
    def getHappyString(self, n: int, k: int) -> str:
        def generate(prev):
            if len(prev) == n:
                yield prev
                return
            for c in 'abc':
                if not prev or c != prev[-1]:
                    yield from generate(prev + c)
            
        for i, res in enumerate(generate(''), 1):            
            if i == k:
                return res
        return ''  

```


