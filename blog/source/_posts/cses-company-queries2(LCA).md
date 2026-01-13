---
title: CSES Company Queries II (LCA)
date: 2026-01-06
category: [算法, LCA]
tags: [LCA]

---
# company-queries2(LCA)
## 题意
计算树上两个节点的最近公共祖先

## 题解
这里用倍增法求
1. 算出两个节点的深度和倍增数组
2. 算出两个节点的深度差，然后让深度大的节点向上跳到和深度小的节点同一高度
3. 然后让两个节点同时向上跳，直到它们相等，即为最近公共祖先


## 参考代码
```cpp
#include<bits/stdc++.h>
using namespace std;

// ... 你的宏定义 ...

const int N = 2e5 + 10;
int n, q;
int dep[N], dp[N][20];
vector<int> adj[N];

void dfs(int x, int fa){
    dep[x] = dep[fa] + 1;
    for(auto c : adj[x]){
        // 因为你的输入保证了父子关系，这里不需要判断 c == fa
        // 但为了严谨，通常会写上
        dfs(c, x);
    }
}

int LCA(int x, int y) {
    if(dep[x] < dep[y]) swap(x, y);

    // 1. 对齐深度
    int dec = dep[x] - dep[y];
    for(int i = 19; i >= 0; i--){ // 建议从高位往低位跳，更稳健
        if((dec >> i) & 1) x = dp[x][i];
    }

    if(x == y) return y;

    // 2. 倍增向上跳
    for(int i = 19; i >= 0; i--) {
        if(dp[x][i] != dp[y][i]) {
            x = dp[x][i]; 
            y = dp[y][i];
        }
    }
    return dp[x][0];
}

signed main() {
    ios::sync_with_stdio(0); cin.tie(0); // 别忘了加速

    if(!(cin >> n >> q)) return 0;
    
    for(int i = 2; i <= n; ++i){ // 习惯从 2 开始
        int p; cin >> p;
        dp[i][0] = p;
        adj[p].push_back(i);
    }

    // --- 核心修复点 ---
    dep[0] = 0; // 确保虚拟节点的深度为0
    dfs(1, 0); 
    // -----------------

    // 预处理倍增表
    for(int j = 1; j < 20; ++j){
        for(int i = 1; i <= n; ++i) {
            if(dp[i][j-1] != 0)
                dp[i][j] = dp[dp[i][j-1]][j-1];
        }
    }

    while(q--){
        int x, y; cin >> x >> y;
        cout << LCA(x, y) << '\n'; // 换成 \n
    }
    return 0;
}
```
