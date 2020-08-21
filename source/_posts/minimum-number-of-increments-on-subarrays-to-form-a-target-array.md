---
title: Minimum Number Of Increments On Subarrays To Form A Target Array's Solution
date: 2020-07-29 22:56:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Minimum Number Of Increments On Subarrays To Form A Target Array](https://leetcode.com/problems/minimum-number-of-increments-on-subarrays-to-form-a-target-array)
**Original Solution Post**: [Solution](https://leetcode.com/problems/minimum-number-of-increments-on-subarrays-to-form-a-target-array/discuss/754630/C%2B%2BPython-1-line-Greedy-Solution)

**Idea**<br>
Traversing through the array is like travelling through a collection of mountains.<br>
Whenever we are going downwards, we have to remove the peak we've passed.<br>
It is obvious that a single peak of height `h` can be taken down with `h` movements.




Please upvote if you find this post helpful or interesting. It means a lot to me. Thx~




**Complexity**




- Time: `O(n)`
- Space: Constant



**Python**




```
class Solution:
    def minNumberOperations(self, target: List[int]) -> int:
        return sum(max(target[i - 1] - target[i], 0) for i in range(1, len(target))) + target[-1]

```



**C++**




```
class Solution {
public:
    int minNumberOperations(vector<int>& target) {
        int res = 0;
        vector<int> increase;
        target.push_back(0);
        for (auto x: target){
            if (!increase.empty() && increase.back() > x)
                res += increase.back() - x;
            while (!increase.empty() && increase.back() >= x)
                increase.pop_back();
            increase.push_back(x);
        }
        return res;
    }
};

```


