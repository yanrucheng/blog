---
title: Constrained Subsequence Sum's Solution
date: 2020-04-26 12:13:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum)
**Original Solution Post**: [Solution](https://leetcode.com/problems/constrained-subsequence-sum/discuss/597693/PythonC%2B%2B-DP-with-decreasing-deque)

**Idea**<br>
This is a typical knapsack problem. we maintain an array `dp`, where `dp[i]` is the maximum sum we can get from nums[:i] and nums[i] is guaranteed to be included.




- Base case: `dp[0] = nums[0]`
<li>state transition: `dp[i] = max(dp[i - k], dp[i-k+1], ..., dp[i - 1], 0) + x`
<ul>
- NOTE that x can be a fresh start when all the previous dp are negative.



This algorithm is only `O(n * k)`, we need to improve it to `O(n)` because both `k` and `n` can be 10^5.




The Idea is straight-forward, we can maintain an non-increasing deque `decrease` that records the maximum value among `dp[i - k], dp[i-k+1], ..., dp[i - 1]`. When encountering a new value `x`, we only record it in `decrease` if `x > decrease[decrease.size - 1]`. Thus the first element in `decrease` will always be the largest value we want.




**Complexity**<br>
Time: `O(n)`<br>
Space: `O(n)`




**Python**




```
class Solution:
    def constrainedSubsetSum(self, nums: List[int], k: int) -> int:
        dp = nums[:1]
        decrease = collections.deque(dp)
        for i, x in enumerate(nums[1:], 1):
            if i > k and decrease[0] == dp[i - k - 1]:
                decrease.popleft()
            tmp = max(x, decrease[0] + x)
            dp += tmp,
            while decrease and decrease[-1] < tmp:
                decrease.pop()
            decrease += tmp,                
        return max(dp)  

```



**C++**




```
class Solution {
public:
    int constrainedSubsetSum(vector<int>& nums, int k) {
        vector<int> dp {nums[0]};
        deque<int> decrease {nums[0]};
        int res = nums[0];
        
        for (int i=1; i<nums.size(); i++) {
            if (i > k && decrease[0] == dp[i - k - 1])
                decrease.pop_front();
            int tmp = max(nums[i], decrease[0] + nums[i]);
            dp.push_back(tmp);
            while (!decrease.empty() && decrease.back() < tmp)
                decrease.pop_back();
            decrease.push_back(tmp);
            
            res = max(res, tmp);
        }
        return res;
        
    }
};

```


