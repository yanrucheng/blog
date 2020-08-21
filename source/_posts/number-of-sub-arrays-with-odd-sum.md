---
title: Number Of Sub Arrays With Odd Sum's Solution
date: 2020-07-26 00:11:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Number Of Sub Arrays With Odd Sum](https://leetcode.com/problems/number-of-sub-arrays-with-odd-sum)
**Original Solution Post**: [Solution](https://leetcode.com/problems/number-of-sub-arrays-with-odd-sum/discuss/754721/C%2B%2BPython-7-line-Intuitive-Constant-Space-DP)

**Idea**<br>
This is an elementary dynamic programming problem.<br>
`odd[i]` records the number of subarray ending at `arr[i]` that has odd sum.<br>
`even[i]` records the number of subarray ending at `arr[i]` that has even sum.<br>
if arr[i + 1] is odd, `odd[i + 1] = even[i] + 1` and `even[i + 1] = odd[i]`<br>
if arr[i + 1] is even, `odd[i + 1] = odd[i]` and `even[i + 1] = even[i] + 1`<br>
Since we only required the previous value in `odd` and `even`, we only need `O(1)` space.




Please upvote if you find this post helpful or interesting. It means a lot to me. Thx~




**Complexity**




- Time: `O(n)`
- Space: `O(1)`



**Python**




```
class Solution:
    def numOfSubarrays(self, arr: List[int]) -> int:
        res = odd = even = 0
        for x in arr:
            even += 1
            if x % 2:
                odd, even = even, odd
            res = (res + odd) % 1000000007             
        return res            

```



**C++**




```
class Solution {
public:
    int numOfSubarrays(vector<int>& arr) {
        int res = 0, odd = 0, even = 0;
        for (auto x: arr) {
            even += 1;
            if (x % 2)
                swap(odd, even);
            res = (res + odd) % 1000000007;
        }
        return res;
    }
};

```


