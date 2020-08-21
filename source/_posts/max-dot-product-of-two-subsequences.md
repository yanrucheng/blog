---
title: Max Dot Product Of Two Subsequences's Solution
date: 2020-05-24 12:13:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Max Dot Product Of Two Subsequences](https://leetcode.com/problems/max-dot-product-of-two-subsequences)
**Original Solution Post**: [Solution](https://leetcode.com/problems/max-dot-product-of-two-subsequences/discuss/648261/cpython-simple-dp)

**Idea**<br>
This is a typical Dynamic Programming problem. The only trick lies in the **non-emptiness**.




- Target: We want to calculate the maximal dot product for nums1[0:i] and nums2[0:j]
<li>Base case
<ul>
- When `i == 0` or `j == 0`, we return -inf. (Because this is an **empty** case, which is intolerable)

- `nums1[i - 1]` is not selected, `dp[i][j] = dp[i - 1][j]`
- `nums2[j - 1]` is not selected, `dp[i][j] = dp[i][j - 1]`
- Neither `nums1[i - 1]` or `nums2[j - 1]` is selected, `dp[i][j] = dp[i - 1][j - 1]`
<li>Both `nums1[i - 1]` and `nums2[j - 1]` are selected, `dp[i][j] = max(dp[i - 1][j - 1], 0) + nums1[i - 1] * nums2[j - 1]`
<ul>
- **Since we already selected one pair (nums1[i - 1], nums2[j - 1]), we can assume the minimal proceeding value is `0`**



**Complexity**




- Time: `O(mn)`
- Space: `O(m)`



**C++, dp**




```
class Solution {
public:
    int maxDotProduct(vector<int>& nums1, vector<int>& nums2) {
        int n = int(nums1.size()), m = int(nums2.size());
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, INT_MIN));
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= m; ++j) {
                dp[i][j] = max(dp[i][j], dp[i - 1][j]);
                dp[i][j] = max(dp[i][j], dp[i][j - 1]);
                dp[i][j] = max(dp[i][j], dp[i - 1][j - 1]);
                dp[i][j] = max(dp[i][j], max(dp[i - 1][j - 1], 0) + nums1[i - 1] * nums2[j - 1]);
            }
        }
        return dp[n][m];
    }
};

```



**Python 3, recursion with memoization**




```
from functools import lru_cache
class Solution:
    def maxDotProduct(self, nums1: List[int], nums2: List[int]) -> int:
        @lru_cache(None)
        def helper(i, j):
            if i == 0 or j == 0: return -math.inf
            return max(helper(i - 1, j - 1), helper(i, j - 1), helper(i - 1, j),
                       max(helper(i - 1, j - 1), 0) + nums1[i - 1] * nums2[j - 1])
        return helper(len(nums1), len(nums2))

```



**Python 3, dp**




```
class Solution:
    def maxDotProduct(self, nums1: List[int], nums2: List[int]) -> int:
        n, m = len(nums1), len(nums2)
        dp = [-math.inf] * (m + 1)
        for i in range(1, n + 1):
            dp, old_dp = [-math.inf], dp
            for j in range(1, m + 1):
                dp += max(
                    old_dp[j], # not select i
                    dp[-1], # not select j
                    old_dp[j - 1], # not select either
                    max(old_dp[j - 1], 0) + nums1[i - 1] * nums2[j - 1], # select both
                ),
        return dp[-1]     

```


