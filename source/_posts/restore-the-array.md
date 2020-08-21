---
title: Restore The Array's Solution
date: 2020-04-19 00:04:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Restore The Array](https://leetcode.com/problems/restore-the-array)
**Original Solution Post**: [Solution](https://leetcode.com/problems/restore-the-array/discuss/585554/C%2B%2BPython-Simple-DP-with-python)

**Idea**<br>
`dp[i]` records the **number of possible array that can be printed as a string s[i:]**<br>
suppose the length of `s` is `n`.




<li>Base case:
<ul>
- we know that there is exactly **one** way to construct an empty string, thus `dp[n] = 1`

- find out all the possible number we can input starting at s[i], thus `dp[i] = sum(dp[j] for all valid j)` where a valid `j` means `1 <= int(s[i:j]) <= k`



**Complexity**<br>
TIme: `O(n)`<br>
Space: `O(n)`, could be optimised to O(#digits of k)




**C++, without using long**




```
class Solution {
public:
    int numberOfArrays(string s, int k) {
        int n = (int)s.size();
        vector<int> dp(n + 1, 0);
        dp[n] = 1; // the base case
        for (int i=n-1; i>=0; --i) {
            int num = s[i] - '0', j = i + 1;
            while (num > 0 && num <= k && j < n + 1) {
                dp[i] = (dp[i] + dp[j]) % 1000000007;
		        // a tiny trick to avoid overflow
                num = (j < n && num <= k / 10) ? 10 * num + (s[j] - '0') : INT_MAX;
                j++;
            }
        }
        return dp[0];
    }
};

```



**Python 3**




```
class Solution:
    def numberOfArrays(self, s: str, k: int) -> int:
        n = len(s)
        s = [*map(int, s)] + [math.inf] # for easier implementation
        dp = [0] * n + [1]
        for i in range(n - 1, -1, -1):
            num, j = s[i], i + 1
            while 1 <= num <= k and j < n + 1:
                dp[i] = (dp[i] + dp[j]) % 1000000007
                num = 10 * num + s[j]
                j += 1
        return dp[0]

```


