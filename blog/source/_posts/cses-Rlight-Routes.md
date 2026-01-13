---
title: CSES - Rlight Routes(K Shortest Paths)
date: 2026-01-01
category: [算法, k短路]
tags: CSES

---
# CSES - Rlight Routes
## 题意
求单向图的k短路

## 题解
使用A*算法, 设评估函数$f(x) = g(x) + h(x)$, 其中$g(x)$为从起点到$x$的最短距离, $h(x)$为从$x$到终点的最短距离的估计值
其中$h(x)$为从$x$到终点的最短距离的估计值, 可以使用Dijkstra算法, 通过建立的反向边算出来
每次选出$f(x)$最小的点, 遍历邻接边更新, 注意更新的逻辑关系和剪枝
**剪枝**: 当一个点弹出超过$k$次了, 就不用处理了, 因为在$k+1$次的时候如果到达终点是最短路的话, 前$k$次也一定是最短路.



## 参考代码
```cpp

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
struct Edge{
    int to, w;
    Edge(){}
    Edge(int to, int w):to(to), w(w){}
};

struct Node{
    int u, dis, g;  //起点经过节点u到终点的距离dis, 节点u到起点的距离g
    Node(){}
    Node(int u, int dis):u(u), dis(dis){}
    Node(int u, int dis, int g):u(u), dis(dis), g(g){}
    bool operator < (const Node& a) const {
        return dis > a.dis;
    }
};

int dis[N];  //反向距离, 也就是到终点的距离
int n, m, k;
bool vis[N];
vector<Edge> edge[N], redge[N];  //正向边和反向边
void dijkstra(){
    memset(dis, 0x3f, sizeof(dis));
    priority_queue<Node, vector<Node>> q;
    q.push({n, 0}); dis[n] = 0;

    while(!q.empty()) {
        int u = q.top().u, dist = q.top().dis; q.pop();
        if(vis[u]) continue;
        vis[u] = 1;

        for(auto [to, w] : redge[u]) {
            if(dis[to] > dis[u] + w) {
                dis[to] = dis[u] + w;
                q.push({to, dis[to]});
            }
        }
    }
}

int cnt[N], ans[N];  //记录出队次数和答案
void Astar(){
    priority_queue<Node, vector<Node>> q;
    q.push({1, dis[1], 0});

    while(!q.empty()) {
        int u = q.top().u; int dist = q.top().dis;int g = q.top().g; q.pop();
        if(dis[u] == dis[0]) continue;  //不能到达终点的点
        cnt[u]++;  //记录次数
        if(u == n) ans[cnt[u]] = dist;  //记录答案
        if(cnt[n]>=k) break;  //如果已经找到k个答案，退出循环
        if(cnt[u]>k) continue; //如果次数超过k，跳过

        for(auto [to, w] : edge[u]) {
            q.push({to, g+w +dis[to], g+w}); //更新距离和g值
        }
    }
}

signed main() {
    caillo;

    cin >> n >> m >> k;
    fi(m){
        int u, v, w;
        cin >> u >> v >> w;
        edge[u].push_back({v, w});
        redge[v].push_back({u, w});
    }

    dijkstra();
    Astar();
    for(int i= 1;i <= k; ++i) cout << ans[i] << ' ';
}

```
