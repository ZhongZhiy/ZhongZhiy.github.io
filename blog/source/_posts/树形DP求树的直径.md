---
title: 树形DP求树的直径
data: 2026-01-07
categories: [算法, 树的直径]
tags: 树的直径
---


定义状态`dp[u]`为以`u`为根节点的子树上从`u`出发能够到达的最远路径长度, 这个长度是`u`的一个叶子节点.

状态转移: 设`u`有`t`个直连的子节点$v_{1}, v_{2}, v_{3}, \dots v_{t}$, 那么`dp[u]`的值为
$$dp[u] = max\{dp[v_{i}]+edge(u, v{i})\}, 1\leq i \leq t$$

记`f[u]`为过点`u`的最长路径长度, 那么
$$f[u] = max\{dp[u] + dp[v_{i}] + edge(u, v_{i})\}$$
其中`dp[u]`为不包括$v_i$的以`u`为根所能到达的最长路径长度


最后答案就是$max\{f[u]\}$

```cpp
void dfs(int u) {
	for(auto c : adj[u]) {
		dfs(v);
		//这里就先算f[u], 而不先更新dp[u]
		maxlen = max(maxlen, dp[u] + dp[c.id] + c.edge);
		dp[u] = max(dp[u], dp[c.id] + c.edge);
	}

}
```
