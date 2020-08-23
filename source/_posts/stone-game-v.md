---
title: Stone Game V's Solution
date: 2020-08-23 11:42:05
categories: Leetcode
tags: Algorithm
---

Original Question: [Stone Game V](https://leetcode.com/problems/stone-game-v/)
Original Solution Post: [Solution](https://leetcode.com/problems/stone-game-v/discuss/806736/Python-Golang-Simple-DP-Solution-with-a-Trick-for-Python)

# Question

## Stone Game V
There are several stones **arranged in a row**, and each stone has an associated value which is an integer given in the array stoneValue.

In each round of the game, Alice divides the row into **two non-empty rows** (i.e. left row and right row), then Bob calculates the value of each row which is the sum of the values of all the stones in this row. Bob throws away the row which has the maximum value, and Alice's score increases by the value of the remaining row. If the value of the two rows are equal, Bob lets Alice decide which row will be thrown away. The next round starts with the remaining row.

The game ends when there is only one stone remaining. Alice's is initially zero.

Return the maximum score that Alice can obtain.

### Example 1:

**Input:** stoneValue = [6,2,3,4,5,5]
**Output:** 18
**Explanation:** In the first round, Alice divides the row to [6,2,3], [4,5,5]. The left row has the value 11 and the right row has value 14. Bob throws away the right row and Alice's score is now 11.
In the second round Alice divides the row to [6], [2,3]. This time Bob throws away the left row and Alice's score becomes 16 (11 + 5).
The last round Alice has only one choice to divide the row which is [2], [3]. Bob throws away the right row and Alice's score is now 18 (16 + 2). The game ends because only one stone is remaining in the row.

### Example 2:
**Input:** stoneValue = [7,7,7,7,7,7,7]
**Output:** 28

### Example 3:
**Input:** stoneValue = [4]
**Output:** 0

### Constraints:
- `1 <= stoneValue.length <= 500`
- `1 <= stoneValue[i] <= 10^6`



# [Python / Golang] Simple DP Solution with a Trick for Python

## Idea
The idea is straight forward. we just try out all the possibilities to find the answer. Naive brute force solution will lead to O(n!) complexity, so we need to store the interim results to improve the complexity to O(n^3).

## A trick for slower languages
It turns out that O(n^3) is not enough for dynamic languages like Python. So I add a shortcut for a special case when all the elements within a specific range are the same. In this special case, we can get the results in O(logn) complexity.

## Complexity
Time: `O(n^3)`
Space: `O(n^2)`


## Python
```
from functools import lru_cache
class Solution:
    def stoneGameV(self, stoneValue: List[int]) -> int:
        presum = [0]
        for s in stoneValue:
            presum += presum[-1] + s,
            
        def sm(i, j):
            return presum[j + 1] - presum[i]
        
        @lru_cache(None)
        def helper(i, j):
            if i == j: return 0
            
            if all(stoneValue[k] == stoneValue[j] for k in range(i, j)):
                cnt = 0
                l = j - i + 1
                while l > 1:
                    l //= 2
                    cnt += l
                return cnt * stoneValue[j]                    
            
            res = 0
            for k in range(i, j):
                if sm(i, k) < sm(k + 1, j):
                    score = helper(i, k) + sm(i, k)
                elif sm(i, k) > sm(k + 1, j):
                    score = sm(k + 1, j) + helper(k + 1, j)
                else:                    
                    score = max(helper(i, k) + sm(i, k), sm(k + 1, j) + helper(k + 1, j))
                res = max(res, score)
            return res                
        
        return helper(0, len(stoneValue) - 1)
```


## Golang
```
func stoneGameV(stoneValue []int) int {
    n := len(stoneValue)
    memo := make([][]int, n)
    for i := range memo {
        memo[i] = make([]int, n)
    }
    presum := make([]int, n + 1)
    for i := 1; i <= n; i++ {
        presum[i] = presum[i - 1] + stoneValue[i - 1]
    }
    sum := func(i, j int) int {return presum[j + 1] - presum[i]}
    
    
    for l := 2; l <= n; l++ { // l mean length
        for i := 0; i < n - l + 1; i++ {
            j := i + l - 1
            score := 0
            for k := i; k < j; k++ {
                switch {
                case sum(i, k) > sum(k + 1, j):
                    score = max(score, memo[k + 1][j] + sum(k + 1, j))
                case sum(i, k) < sum(k + 1, j):
                    score = max(score, memo[i][k] + sum(i, k))
                default:
                    tmp := max(memo[i][k] + sum(i, k), memo[k + 1][j] + sum(k + 1, j))
                    score = max(score, tmp)
                }
            }
            memo[i][j] = score
        }
    }
    return memo[0][n - 1]
}

func max(i, j int) int {
    if i < j {
        return j
    }
    return i
}
```
