---
title: Leetcode Contest 410 Solutions
summary: A concise guide to solving Leetcode Contest 410 problems with optimized strategies and clear explanations.
date: 2024-08-14
authors:
  - admin
tags:
  - leetcode
  - algorithm
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

```cpp
class Solution:
    def finalPositionOfSnake(self, n: int, commands: List[str]) -> int:
        grid = [[0 for _ in range(n)] for _ in range(n)]
        idx = 0
        for i in range(n):
            for j in range(n):
                grid[i][j] = idx;
                idx += 1;
        r = 0
        c = 0
        for i in range(len(commands)):
            cm = commands[i]
            if cm == "UP":
                r -= 1
            elif cm == "RIGHT":
                c += 1
            elif cm == "DOWN":
                r += 1
            else:
                c -= 1

        return grid[r][c]
```

```python
class Solution:
    def countGoodNodes(self, edges: List[List[int]]) -> int:

        n = len(edges) + 1
        graph = [[] for _ in range(n)]

        for [a, b] in edges:
            graph[a].append(b)
            graph[b].append(a)


        ret = 0

        def dfs(node, parent):
            nonlocal ret
            cnts = []
            valids= []
            for nxt in graph[node]:
                if nxt == parent:
                    continue
                valid, cnt = dfs(nxt, node)
                valids.append(valid)
                cnts.append(cnt)

            if len(set(cnts)) <= 1:
                ret += 1
                return True, 1 + sum(cnts)
            else:
                return False, 1 + sum(cnts)



        dfs(0, -1)
        return ret

```

```python
class Solution:
    def countOfPairs(self, nums: List[int]) -> int:
        MOD = int(1e9 + 7)
        n = len(nums)
        m = max(nums) + 1

        dp = [[0 for _ in range(m + 1)] for _ in range(n + 1)]
        for i in range(n):
            for j in range(m + 1):
                if i == 0:
                    if nums[i] - j >= 0:
                        dp[i][j] = 1
                    else:
                        dp[i][j] = 0
                else:
                    for k in range(j + 1):
                        if nums[i - 1] - k >= nums[i] - j and nums[i] - j >= 0:
                            dp[i][j] = (dp[i][j] + dp[i - 1][k]) % MOD

        return sum(dp[n - 1]) % MOD
```

```python
class Solution:
    def countOfPairs(self, nums: List[int]) -> int:
        MOD = int(1e9 + 7)
        n = len(nums)
        m = max(nums) + 1

        dp = [[0 for _ in range(m + 1)] for _ in range(n + 1)]
        p1 = [0 for _ in range(m + 2)]
        p2 = [0 for _ in range(m + 2)]
        for i in range(n):
            for j in range(m + 1):
                if i == 0:
                    if nums[i] - j >= 0:
                        dp[i][j] = 1
                    else:
                        dp[i][j] = 0
                else:
                    k = nums[i - 1] - nums[i] + j
                    if nums[i] - j >= 0:
                        if k >= m:
                            k = m
                        if k < 0:
                            dp[i][j] = 0
                            continue
                        if k >= j:
                            k = j
                        dp[i][j] = p1[k]
                    else:
                        dp[i][j] = 0

                if j != 0:
                    p2[j] = (p2[j - 1] + dp[i][j]) % MOD
                else:
                    p2[j] = (0 + dp[i][j]) % MOD

            p1 = list(p2)
            p2 = [0 for _ in range(m + 2)]



        return sum(dp[n - 1]) % MOD




```
