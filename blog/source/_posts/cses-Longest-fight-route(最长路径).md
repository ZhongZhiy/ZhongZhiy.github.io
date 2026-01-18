---
title: CSES - Longest Fight Route (最长路径)
date: 2026-01-11
categories: [算法, 最长路径]
tags: CSES

---

# CSES - Longest Fight Route (最长路径)
## 题意
求从节点$1$到节点$n$的最长路径长度。

## 题解
这是**有向无环图**求最长路径问题, 运用DP思想使用**拓扑排序消除后效性**,
定义状态:$dp[i]$表示从节点$1$到节点$i$的最长路径长度。

状态转移:
$$dp[i] = \max_{j \in adj[i]} dp[j] + 1$$
其中$dp[i]$要确保所有到$i$节点的路径都已经计算出来了, 这就需要用到**拓扑排序**, 把每个节点的所有前驱节点都计算出来才能计算

## 参考代码

```cpp

#include <climits>
#include <queue>
#pragma GCC optimize("Ofast")
#include<bits/stdc++.h>
using namespace std;
#define i128 __int128


#ifdef DEBUG
#define de(x) cout << (#x) << " = " << (x) << endl;
#define de2(x, y) cout << (#x) << " , " << (#y) << " = " << (x) << " ~ " << (y) << endl;
#else
#define de(x)
#define de2(x, y)
#endif

#define enld endl
#define endl '\n'
#define fi(x) for(int i = 1; i <= x; ++i)
#define fi0(x) for(int i = 0; i < x; ++i)
#define fj(n) for(int j = 1; j <= n; ++j)
#define caillo ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
#define PLEASE_AC return 0
typedef long long ll;
#define int long long

const int N = 1e5 + 10;
int n, m;

int dp[N], pre[N];
vector<int> adj[N];
int ind[N];

void bfs(){
    for(int i = 0;i <= n; ++i) dp[i] = -INT_MAX;
    queue<int> q;
    for(int i = 1;i <= n; ++i) if(!ind[i]) q.push(i);  //对所有入度为0的节点都入队
    pre[1] = 0;
    dp[1] = 1;

    while(!q.empty()) {
        int d = q.front(); q.pop();

        for(auto c : adj[d]) {
            ind[c]--;
            if(dp[d] > 0) {  //只有从起点出发的才可以转移!!
                if(dp[c] < dp[d] + 1) {
                    dp[c] = dp[d] + 1;
                    pre[c] = d;
                }
            }
            if(!ind[c]) q.push(c);
        }
    }
}

void printl(){
    // for(int i = 1;i <= n; ++i) cout << dp[i] <<  ' ';
    if(dp[n] < 1) cout << "IMPOSSIBLE\n";
    else {
        cout << dp[n] << endl;
        vector<int> ans;
        int x = n;
        while(x != 0){
            ans.push_back(x);
            x = pre[x];
        }
        reverse(ans.begin(), ans.end());
        for(auto c : ans) cout << c << ' ';
    }
}

signed main() {
    caillo;
    cin >> n>> m;
    fi(m) {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        ind[v]++;
    }

    bfs();
    printl();
}

```
