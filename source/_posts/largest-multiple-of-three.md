---
title: Largest Multiple Of Three's Solution
date: 2020-02-23 14:27:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Largest Multiple Of Three](https://leetcode.com/problems/largest-multiple-of-three)
**Original Solution Post**: [Solution](https://leetcode.com/problems/largest-multiple-of-three/discuss/517570/python-on-simple-bucket-sort-with-explanation)

**Idea**<br>
According to the **3 divisibility rule** (proof: [https://math.stackexchange.com/questions/341202/how-to-prove-the-divisibility-rule-for-3-casting-out-threes](https://math.stackexchange.com/questions/341202/how-to-prove-the-divisibility-rule-for-3-casting-out-threes)).<br>
Our goal is to find **the maximum number of digits whose sum is a multiple of 3**.




We categorize all the input digits into 3 category.




- `{0, 3, 9}`: these digits should be accepted into our candidates no matter what
- `{1, 4, 7}`: **category 1**, these digits can only be accepted into out candidates **3 in a group**, or together with 1 element from **category 2**
- `{2, 5, 8}`: **category 2**, these digits can only be accepted into out candidates **3 in a group**, or together with 1 element from **category 1**



Edge case: `[1,1,1,2,2]` and `[1,1,1,2]`. We need to distinguish these 2 edge cases.




<li>in cases like `[1,1,1,2]`
<ul>
- we need to take from **category 1** and **category 2** 3 elements in a group first

- we need to take 1 element from both **category 1** and **category 2** first



**Complexity**<br>
TIme: `O(n)` using bucket sort<br>
Space: `O(n)`




**Python 3, Bucket Sort, O(n) in time and space, more readable but longer**




```
class Solution:
    def largestMultipleOfThree(self, digits: List[int]) -> str:
        def bucket_sorted(arr):
            c = collections.Counter(arr)
            return [x for i in range(10) if i in c for x in [i]*c[i]]
        
        d = collections.defaultdict(list)
        for n in digits:
            d[n%3] += n,
        res, short, long = d[0], *sorted((d[1], d[2]), key=len)
        short, long = bucket_sorted(short), bucket_sorted(long)
        
        if (len(long) - len(short)) % 3 > abs((len(long) % 3) - (len(short) % 3)):
            short, long = (short, long) if len(short)%3 < len(long)%3 else (long, short)
            res += short + long[abs(len(short)%3 - len(long)%3):]
        else:
            res += short + long[(len(long)-len(short))%3:]
            
        res = bucket_sorted(res)[::-1]
        return '0' if res and not res[0] else ''.join(map(str, res))

```



**Python 3, using reduce and default sort, O(nlogn)**




```
from functools import reduce
class Solution:
    def largestMultipleOfThree(self, digits: List[int]) -> str:        
        d, _ = reduce(lambda pair,y:(pair[0],pair[0][y%3].append(y)), digits, (collections.defaultdict(list), None))
        res, short, long = d[0], *sorted([sorted(arr) for arr in (d[1], d[2])], key=len)
        
        if (len(long) - len(short)) % 3 > abs((len(long) % 3) - (len(short) % 3)):
            short, long = (short, long) if len(short)%3 < len(long)%3 else (long, short)
            res += short + long[abs(len(short)%3 - len(long)%3):]
        else:
            res += short + long[(len(long)-len(short))%3:]
            
        res = res.sort(reverse=True)
        return '0' if res and not res[0] else ''.join(map(str, res))

```


