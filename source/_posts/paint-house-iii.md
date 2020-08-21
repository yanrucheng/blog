---
title: Paint House Iii's Solution
date: 2020-06-07 12:23:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Paint House Iii](https://leetcode.com/problems/paint-house-iii)
**Original Solution Post**: [Solution](https://leetcode.com/problems/paint-house-iii/discuss/674298/C%2B%2BPython-Simple-DP-top-down-bottom-up)

**Complexity**<br>
Time: `O(n * n * m * target)`<br>
Space: `O(n * m * target) (can be optimized to O(n * target))`




**C++, bottom-up, DP**




```
class Solution {
public:
    int minCost(vector<int>& houses, vector<vector<int>>& cost, int m, int n, int target) {
        int INF = 1000000;
        vector<vector<vector<int>>> dp(m, vector<vector<int>>(target + 1, vector<int>(n, INF)));
            
        for (int c = 1; c <= n; c++)
            if (houses[0] == c)
                dp[0][1][c - 1] = 0;
            else if (!houses[0])
                dp[0][1][c - 1] = cost[0][c - 1];
        
        for (int i = 1; i < m; i++)
            for (int k = 1; k <= min(target, i + 1); k++)
                for (int c = 1; c <= n; c++) {
                    if (houses[i] && c != houses[i]) continue;
                    int same_neighbor = dp[i - 1][k][c - 1];
                    int diff_neighbor = INF;
                    for (int c_ = 1; c_ <= n; c_++)
                        if (c_ != c)
                            diff_neighbor = min(diff_neighbor, dp[i - 1][k - 1][c_ - 1]);
                    int paint_cost = cost[i][c - 1] * int(!houses[i]);
                    dp[i][k][c - 1] = min(same_neighbor, diff_neighbor) + paint_cost;
                }
        int res = *min_element(dp.back().back().begin(), dp.back().back().end());
        return (res < INF) ? res : -1;            
    }
};

```



**Python, bottom-up, DP**




```
class Solution:
    def minCost(self, houses: List[int], cost: List[List[int]], m: int, n: int, target: int) -> int:
        # dp[i][c][k]: i means the ith house, c means the cth color, k means k neighbor groups
        dp = [[[math.inf for _ in range(n)] for _ in range(target + 1)] for _ in range(m)]
        
        for c in range(1, n + 1):
            if houses[0] == c: dp[0][1][c - 1] = 0
            elif not houses[0]: dp[0][1][c - 1] = cost[0][c - 1]
                
        for i in range(1, m):
            for k in range(1, min(target, i + 1) + 1):
                for c in range(1, n + 1):
                    if houses[i] and c != houses[i]: continue
                    same_neighbor_cost = dp[i - 1][k][c - 1]
                    diff_neighbor_cost = min([dp[i - 1][k - 1][c_] for c_ in range(n) if c_ != c - 1] or [math.inf])
                    paint_cost = cost[i][c - 1] * (not houses[i])
                    dp[i][k][c - 1] = min(same_neighbor_cost, diff_neighbor_cost) + paint_cost
        res = min(dp[-1][-1])
        return res if res < math.inf else -1

```



**Python, top-down, recursion**




```
from functools import lru_cache
class Solution:
    def minCost(self, houses: List[int], cost: List[List[int]], m: int, n: int, target: int) -> int:
        
        @lru_cache(None)
        def helper(i, k, c):
            # min cost for paint the ith house in color c, resulting k neighborhood
            if i < 0 and k <= 1: return 0
            if k > i + 1 or k < 1: return math.inf
            if houses[i] and c != houses[i]: return math.inf
            
            paint_cost = cost[i][c - 1] * (not houses[i])
            prev_cost = min(helper(i - 1, k, c), min([helper(i - 1, k - 1, c_) \
                    for c_ in range(1, n + 1) if c_ != c] or [math.inf]))
            
            return paint_cost + prev_cost
        
        res = min(helper(m - 1, target, c) for c in range(1, n + 1))
        return res if res < math.inf else -1

```


