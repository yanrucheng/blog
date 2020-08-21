---
title: Count All Valid Pickup And Delivery Options's Solution
date: 2020-02-23 08:56:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Count All Valid Pickup And Delivery Options](https://leetcode.com/problems/count-all-valid-pickup-and-delivery-options)
**Original Solution Post**: [Solution](https://leetcode.com/problems/count-all-valid-pickup-and-delivery-options/discuss/516933/C%2B%2BPython-1-line-Simple-permutation-with-explanation)

**Idea**<br>
Denote `pickup 1, pickup 2, pickup 3, ...` as `A, B, C, ...`<br>
Denote `delivery 1, delivery 2, delivery 3, ...` as `a, b, c, ...`<br>
We need to ensure `a` is behind `A`, `b` is behind `B`, ...




This solution involves  2 stages.




<li>Stage 1
<ul>
- We decide the order of all the pickups. It is trivial to tell there are `n!` possibilities

- Given one possibility. Let's say the pickups are ordered  like this `A B C`
<li>We can now insert the corresponding deliveries one by one.
<ul>
<li>We start with the last pickup we made, namely, insert `c`, and there is only 1 valid slot.
<ul>
- `A B C c`

- `A B` **x** `C` **x** `c` **x** (where **x** denotes the location of valid slots for `b`)

- `A` **x** `B` **x** `C` **x** `c` **x** `b` **x**, (where **x** denotes the location of valid slots for `a`)



**Complexity**<br>
Time: `O(n)`, can be improved to `O(1)` with memoization<br>
Space: `O(1)`




**C++**




```
int countOrders(int n) {
    long long res = 1, cap = 1000000007;
    for (int i=1; i<n+1; ++i) res = res * i % cap;
    for (int i=1; i<2*n; i+=2) res = res * i % cap;
    return res;
}

```



**Python 3, with explanations and meaningful variable names**




```
from functools import reduce
from operator import mul
class Solution:
    def countOrders(self, n: int) -> int:
        cap = 10**9 + 7
        pickup_permutation = math.factorial(n) % cap
        delivery_permutation = reduce(mul, range(1, 2*n, 2), 1) % cap
        return pickup_permutation * delivery_permutation % cap

```



**Python 3, 1-line, mod along the way, O(1) space (12.8 MB)**




```
from functools import reduce
class Solution:
    def countOrders(self, n: int) -> int:
		# remember to use generator instead of tuple for O(1) space complexity
        return reduce(lambda x, y: x * y % (10**9+7), (v for x in range(1,n+1) for v in (x, 2*x-1)), 1)

```



**Python 3, 1-line, short, O(n) space**




```
from functools import reduce
from operator import mul
class Solution:
    def countOrders(self, n: int) -> int:
        return reduce(mul, (*range(1,n+1), *range(1,2*n,2)), 1) % (10**9+7)

```


