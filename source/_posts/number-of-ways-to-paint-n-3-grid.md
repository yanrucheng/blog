---
title: Number Of Ways To Paint N 3 Grid's Solution
date: 2020-04-12 12:08:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Number Of Ways To Paint N 3 Grid](https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid)
**Original Solution Post**: [Solution](https://leetcode.com/problems/number-of-ways-to-paint-n-3-grid/discuss/574876/PythonC%2B%2B-Simple-state-compression-DP-O(1)-space)

**Idea**<br>
As shown in the first example, there are 12 possible states for a single row and let's name them as `states0, states1, ... states11`. Intuitively we should construct a `dp[i][state]` denoting the number of ways to paint i * 3 grid, with the last row in `state`.<br>
<img src="https://assets.leetcode.com/users/yanrcheng2/image_1586664387.png" alt="image">




We first compute the compatibilities of different states. 2 states are compatible if and only if no adjacent grid between them are in the same colour. The full compatibility list are given below.




**Full Compatibility List:**




```
[
	[1, 2, 4, 5, 10], 
	[0, 3, 6, 8, 11], 
	[0, 3, 6, 7], 
	[1, 2, 7, 10], 
	[0, 6, 8, 9], 
	[0, 6, 7, 9, 10], 
	[1, 2, 4, 5, 11], 
	[2, 3, 5, 11], 
	[1, 4, 9, 10], 
	[4, 5, 8, 11], 
	[0, 3, 5, 8, 11], 
	[1, 6, 7, 9, 10]
]

```



We then have `dp[i][state] = sum(dp[i - 1][state'])` where `state'` are compatible with `state`




**Complexity**<br>
Time: `O(n)`<br>
Space: `O(1)`




**Python**




```
class Solution:
    def numOfWays(self, n: int) -> int:
        MOD = 1000000007
        P = ['ryr', 'yry', 'gry', 'ryg', 'yrg', 'grg', 'rgr', 'ygr', 'gyr', 'rgy', 'ygy', 'gyg']
        nei = [[i for i, x in enumerate(P) if all(a != b for a, b in zip(x, p))] for p in P]
        
        dp = [1] * 12
        for i in range(n - 1):
            dp = [sum(dp[j] for j in nei[i]) % MOD for i in range(12)]
        return sum(dp) % MOD

```



**C++**




```
class Solution {
public:
    int numOfWays(int n) {
        vector<vector<int>> compatibility {
            {1, 2, 4, 5, 10}, 
            {0, 3, 6, 8, 11}, 
            {0, 3, 6, 7}, 
            {1, 2, 7, 10}, 
            {0, 6, 8, 9}, 
            {0, 6, 7, 9, 10}, 
            {1, 2, 4, 5, 11}, 
            {2, 3, 5, 11}, 
            {1, 4, 9, 10}, 
            {4, 5, 8, 11}, 
            {0, 3, 5, 8, 11}, 
            {1, 6, 7, 9, 10}
        };
        auto addMod = [&](int a, int b){return (a + b) % 1000000007;};
        array<int, 12> dp, old;
        dp.fill(1);
        while (--n > 0) {
            old = dp, dp.fill(0);
            for (int i=0; i < 12; ++i)
                for (int j=0; j < compatibility[i].size(); ++j)
                    dp[i] = addMod(dp[i], old[compatibility[i][j]]);
        }
        return accumulate(dp.begin(), dp.end(), 0, addMod);
    }
};

```


