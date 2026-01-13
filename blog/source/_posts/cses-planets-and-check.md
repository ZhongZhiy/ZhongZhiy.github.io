---
title: Planets and check(tarjan)
date: 2026-01-12
categories: [算法, tarjan]
tags: CSES

---

# cses Planets and check(tarjan)
## 题意
$n$个点, $m$条边, 求所有点是不是两两互相可到的, 如果不是, 就输出节点$a$和$b$, 其中$a$不能到达$b$

## 题解
求强连通图的板子, 使用tarjan算法, 经过一次DFS标记就可以分离scc, 并且计算scc, 
基本思想是, 在DFS过程中进行标记, 并且把当前节点入栈, 
对于邻接节点, 如果是没有访问过的, 就进行DFS, 如果访问过, 那么说明这个邻接节点和当前节点之间是一个环, 更新标记

如果当前节点是环的起点, 就进行出栈, 标记这个环scc

对于输出, 如果只有一个联通块, 那么就输出"YES", 否则输出"NO"
找$a$和$b$, 由于是强连通图, 那么就找一个连通块的出度为$0$, 其中的节点可以为$b$

## 参考代码
```cpp
#include <climits>
#include <cstddef>
#include <queue>
#pragma GCC optimize("Ofast")
#include<bits/stdc++.h>
using namespace std;
#define i128 __int128


#define DEBUG
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
int dfn[N], low[N], idfn, in_stack[N];  //dfn是对每个节点进行DFS的顺序标号, low是连通块的根节点标号
int scc[N], sz[N], sc;  //记录每个点的scc标号
vector<int> adj[N];
stack<int> st;
int outd[N];  //记录每个连通块的出度

void tarjan(int u){
    dfn[u] = low[u] = ++idfn;  //先标记
    in_stack[u] = 1;
    st.push(u);
    for(auto v : adj[u]) {
        //对没访问的节点进行DFS, 然后传递连通块根节点
        if(!dfn[v]) {
            tarjan(v);
            low[u] = min(low[u], low[v]);
        }else if(in_stack[v]) {  //如果是访问到访问过的节点, 就说明是一个连通块
            low[u] = min(low[u], dfn[v]);  //这个点就是连通块的根节点
        }
    }

    if(dfn[u] == low[u]) {  //如果当前节点是根
        ++sc;
        while(st.top() != u){  //清空除了根之外的scc节点
            scc[st.top()] = sc;
            sz[sc]++;
            in_stack[st.top()] = 0;
            st.pop();
        }
        //清空这scc的根
        scc[st.top()] = sc;
        sz[sc]++;
        in_stack[st.top()] = 0;
        st.pop();
    }
}

void printl(){
    for(int i = 1;i <= n; ++i) {  //缩点, 计算出度
        for(auto v : adj[i]) {
            if(scc[i] != scc[v]) outd[scc[i]]++;
        }
    }

    int a = 0, b = 0, sca = 0;
    for(int i = 1;i <= sc; ++i) {
        if(!outd[i]) {  //找出度为0的点所在的连通块
            sca = i;
            break;
        }
    }

    for(int i = 1;i <= n; ++i){  //找出度为0的联通块的点和, 除此之外的点
        if(scc[i] == sca){
            a = i;
        }
        if(scc[i] != sca){
            b = i;
        }
    }

    cout << a << ' ' << b << endl;
}

signed main() {
    cin >> n >> m;
    fi(m) {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
    }

    for(int i = 1;i <= n; ++i) {
        if(!scc[i]) tarjan(i);
    }

    if(sc == 1) cout << "YES\n";
    else {
        cout << "NO\n";
        printl();
    }
}

```
