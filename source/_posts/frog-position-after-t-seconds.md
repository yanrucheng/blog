---
title: Frog Position After T Seconds's Solution
date: 2020-03-08 12:21:00
categories:
 - Leetcode
tags:
 - Algorithm
---

**Original Question**: [Frog Position After T Seconds](https://leetcode.com/problems/frog-position-after-t-seconds)
**Original Solution Post**: [Solution](https://leetcode.com/problems/frog-position-after-t-seconds/discuss/532475/python-easy-dfsbfs-with-explanation)

**Idea**<br>
First we build an adjacency list using `edges`.<br>
We then DFS through all the nodes. Do note that we can only visited a node once in an undirected graph. There are basically 2 solutions.




1. Construct a undirected graph and transform it into a directed tree first **(method 1)**
1. Use set to record all the visited nodes along the way **(method 2)**



**Complexity**<br>
Time: `O(N)`<br>
Space: `O(N)`




**Python 3, DFS with recursion, method 2**




```
class Solution:
    def frogPosition(self, n: int, edges: List[List[int]], t: int, target: int) -> float:
        nei = collections.defaultdict(set)
        for a, b in edges:
            nei[a].add(b)
            nei[b].add(a)
            
        visited, res = set(), 0.
        def dfs(leaf_id, p, time):
            nonlocal res
            if time >= t:
                if leaf_id == target: res = p
                return
            visited.add(leaf_id)
            neighbors = nei[leaf_id] - visited
            for n in neighbors or [leaf_id]:
                dfs(n, p / (len(neighbors) or 1), time + 1)
        dfs(1, 1, 0)
        return res

```



**Python 3, BFS without recursion, method 2**




```
class Solution:
    def frogPosition(self, n: int, edges: List[List[int]], t: int, target: int) -> float:
        nei = collections.defaultdict(set)
        for a, b in edges:
            nei[a].add(b)
            nei[b].add(a)
            
        dp = collections.deque([(1, 1, 0)]) # state: leaf_id, possibility, timestamp
        visited = set()
        
        while dp:
            leaf, p, curr = dp.popleft()
            visited.add(leaf)
            
            if curr >= t:
                if leaf == target: return p
                continue
            
            neighbors = nei[leaf] - visited
            for n in neighbors or [leaf]:
                dp += (n, p / (len(neighbors) or 1), curr + 1),
        return 0.

```



**Python 3, DFS without recursion, method 2**




```
class Solution:
    def frogPosition(self, n: int, edges: List[List[int]], t: int, target: int) -> float:
        nei = collections.defaultdict(set)
        for a, b in edges:
            nei[a].add(b)
            nei[b].add(a)
            
        dp = [(1, 1, 0)] # state: leaf_id, possibility, timestamp
        visited = set()
        
        while dp:
            leaf, p, curr = dp.pop()
            visited.add(leaf)
            
            if curr >= t:
                if leaf == target: return p
                continue
            
            neighbors = nei[leaf] - visited
            for n in neighbors or [leaf]:
                dp += (n, p / (len(neighbors) or 1), curr + 1),
        return 0.

```



**Python 3, DFS on tree without recursion, method 1**




```
class Solution:
    def frogPosition(self, n: int, edges: List[List[int]], t: int, target: int) -> float:
        nei = collections.defaultdict(set)
        for a, b in edges:
            nei[a].add(b)
            nei[b].add(a)
                    
        dp = collections.deque([1])
        while dp:
            leaf = dp.popleft()
            for n_ in nei[leaf]:
                nei[n_].remove(leaf)
                dp += n_,
                
        dp = [(1, 1, 0)]
        while dp:
            leaf, p, curr = dp.pop()
            if curr >= t:
                if leaf == target: return p
                continue
            for n in nei[leaf] or [leaf]:
                dp += (n, p / (len(nei[leaf]) or 1), curr+1),
        return 0.0

```


