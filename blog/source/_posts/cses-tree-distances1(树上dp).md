---
title: cses-tree-distances1(树上dp)
date: 2026-1-6
categories: [算法, 树上dp]
tags: 树上dp

---
# tree distances1
## 题意
求树上每个节点与其他节点间的最大举例

## 题解
树上dp, 随便选一个点作为根节点
1. 先自下而上算出每个节点向下的最大深度, 也就是每个得到每个子节点最大深度的情况下得到当前节点的最大深度`f1[i] = max(f1[i], f1[j] + 1)`, 同时记录次大深度
2. 再自上而下, 算每个节点的最大距离, 如果当前节点`i`是父节点最大深度经过的节点, 那么次大深度就可以派上用场了
就这样跑两遍就可以得到每个点的最大距离

[关于换根dp教程](https://zhuanlan.zhihu.com/p/348349531)
## 参考代码
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;
vector<int> adj[N];
int f1[N], f2[N], cho[N], up[N];

// 第一遍 DFS：由子节点更新父节点（自底向上）
void dfs1(int u, int fa) {
    for (int v : adj[u]) {
        if (v == fa) continue;
        dfs1(v, u);
        int d = f1[v] + 1;
        if (d > f1[u]) {
            f2[u] = f1[u];
            f1[u] = d;
            cho[u] = v; // 记录最长路径是经过哪个儿子的
        } else if (d > f2[u]) {
            f2[u] = d;
        }
    }
}

// 第二遍 DFS：由父节点更新子节点（自顶向下）
void dfs2(int u, int fa) {
    for (int v : adj[u]) {
        if (v == fa) continue;
        
        // 计算 v 向上走的最远距离
        if (v == cho[u]) {
            // 如果 v 在 u 的最长路径上，u 只能给 v 提供次长路径或 u 自己的向上路径
            up[v] = max(up[u], f2[u]) + 1;
        } else {
            // 否则，u 可以给 v 提供最长路径或 u 自己的向上路径
            up[v] = max(up[u], f1[u]) + 1;
        }
        
        dfs2(v, u);
    }
}

int main() {
    ios::sync_with_stdio(0); cin.tie(0);
    int n; cin >> n;
    for (int i = 1; i < n; ++i) {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    dfs1(1, 0);
    dfs2(1, 0);

    for (int i = 1; i <= n; ++i) {
        cout << max(f1[i], up[i]) << (i == n ? "" : " ");
    }
    return 0;
}
```
