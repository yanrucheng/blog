---
title: Robot Bounded In Circle's Solution
date: 2019-05-16 10:03:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle)
**Original Solution Post**: [Solution](https://leetcode.com/problems/robot-bounded-in-circle/discuss/293120/python-on-6-line-solution-beats-100)

Tricks:




- Complex number suits this question well.
- Combinations like "LLLL", "RRRR", "RL", "LR" does not have any effect, removing these combinations are considered very efficient in python, which will lead to a ~15% performance gain.

```
class Solution:
    def isRobotBounded(self, ins: str) -> bool:
        ins = ins.replace('LLLL', '').replace('RRRR', '').replace('RL', '').replace('LR', '')
        d, p = 1, 0
        for c in ins:
            if c == 'G': p += d
            else: d *= {'L':1j, 'R':-1j}[c]
        return d.real == 0 or p == 0 or d.real < 0

```


