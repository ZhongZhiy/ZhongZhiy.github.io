---
title: CSES Hamiltonian Flights(状态压缩DP)
date: 2026-01-14
categories: [算法, 状压DP]
tags: CSES

---
 
# CSES Hamiltonian Flights(状态压缩DP)
## 题意
求从$1$到节点$n$有多少条哈密顿通路
$n<=20$

## 题解
$n$只有$20$, 可以枚举, 使用状态压缩DP, 
定义状态$dp[mask][i]$, 其中$mask$为已经访问过的点, $i$为当前访问的点, 
状态转移: $dp[mask][i] = \sum_{v \in S}dp[mask/i][v]$, 当前节点$i$通过所有不含节点$i$且有邻接边的状态转移而来,
初始化: $dp[1][0] = 1$, 起点

## 参考代码
```cpp
#include <bits/stdc++.h>
using namespace std;


const int N = 1e5 + 10;

const int M = 1e9 + 7;
int dp[1<<20][20];
vector<int> adj[20];  //反向边


int main() {
    int n, m;cin >> n >> m;

    for(int i = 0;i < m; ++i) {
        int u, v; cin >> u >> v;
        u--;v--;
        adj[v].push_back(u);
    }

    dp[1][0] = 1;

    for(int mask = 2; mask < (1<<n); mask++) {
        if(!(mask&1)) continue;  //剪枝, 如果 mask 不包含起点，跳过

        //剪枝: 如果提前到达重点,
        if((mask&(1<<(n-1)))&& mask != ((1<<n)-1)) continue;

        for(int u = 0;u < n; ++u) {
            if(!(mask & (1<<u))) continue;

            //寻找上一个节点
            int prev = mask ^ (1 << u);
            for(int v : adj[u]) {
                if(prev & (1<<v)) {
                    dp[mask][u] = (dp[mask][u] + dp[prev][v]) % M;
                }
            }
        }
    }
    cout << dp[(1<<n)-1][n-1] << endl;

}

```
