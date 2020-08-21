---
title: Largest Divisible Subset's Solution
date: 2020-06-13 17:09:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Largest Divisible Subset](https://leetcode.com/problems/largest-divisible-subset)
**Original Solution Post**: [Solution](https://leetcode.com/problems/largest-divisible-subset/discuss/684795/python-onlogn-nsqrtv-beats-100)

**Idea**<br>
First, sort the array `O(nlogn)`.<br>
We maintain a dictionary `d` where `d[v]` records the longest subsequence **ending with v**.<br>
When encountering a new value `v`, `v` can only be appended to those subsequence ending with `v`'s factors. Thus, we only needs to iterate through `v`'s factors, which is `O(sqrt(V))`




**Complexity**




- time: `O(nlogn + n*sqrt(V)), where V is the size of number`
- space: `O(VN)`



**Python3**




```
# factors, O(nlogn + nlogV), V is the size of num in nums
class Solution:
    def largestDivisibleSubset(self, nums: List[int]) -> List[int]:
        def factors(n):
            yield 1
            for i in range(2, int(math.sqrt(n)) + 1):
                if not n % i:
                    yield i
                    if i * i != n:
                        yield n // i
                        
        nums.sort()                        
        d = {}            
        for n in nums:
            for f in factors(n):
                tmp = d.get(f, []) + [n]
                d[n] = tmp if len(d.get(n, [])) < len(tmp) else d.get(n, [])
        return nums and max(d.values(), key=len)
        # AC: 100 ms, beats 100.00%, 13.9 MB

```


