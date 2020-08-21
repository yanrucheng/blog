---
title: Jump Game Iv's Solution
date: 2020-02-09 00:06:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Jump Game Iv](https://leetcode.com/problems/jump-game-iv)
**Original Solution Post**: [Solution](https://leetcode.com/problems/jump-game-iv/discuss/502711/Python-Simple-Solution-with-Explanations)

**Idea**<br>
Start from position 0. Use breadth first search until the last position is found.




**Trick**




1. Track all the values we have explored. For a given value, we only add it to the frontier once
1. Track all the positions we have explored.
1. Python trick: `nei[num] * (num not in num_met)`



**Complexity**<br>
Time: `O(N)`<br>
Space: `O(N)`




**Python 3**




```
def minJumps(self, arr):
    nei = collections.defaultdict(list)
    _ = [nei[x].append(i) for i, x in enumerate(arr)]

    frontier = collections.deque([(0,0)])
    num_met, pos_met = set(), set()
    while frontier:
        pos, step = frontier.popleft() # state: position, step
        if pos == len(arr) - 1: return step
        num = arr[pos]
        pos_met.add(pos) # track explored positions

        for p in [pos - 1, pos + 1] + nei[num] * (num not in num_met):
            if p in pos_met or not 0 <= p < len(arr): continue
            frontier.append((p, step + 1))

        num_met.add(num) # track explored values

```


