---
title: CSES Giant Pizza(2-SAT)
date: 2026-01-14
categories: [算法, 2-SAT]
tags: 2-SAT

---
# CSES Giant Pizza(2-SAT)
## 题意
有$n$条件, 每个条件有$2$个要求, 必须至少满足两个要求中的$1$个, 求存不存在满足所有条件的解

## 题解
这是[2-SAT](https://en.oi-wiki.org/graph/2-sat/)问题, 
条件$(a or b) == (~a and b) or (a and ~b)$这样就可以把两个要求提炼成$~a -> b  or  ~b -> a$的边关系, 
只要在SCC中不出现$a$和$~a$就一定可以满足条件

在 2-SAT 的推导图中，边 $U→V$ 表示逻辑关系：“如果选了 U，就必须选 V”。

如果我们把整个图缩点成一个 DAG（有向无环图），那么边 $SCC_A →SCC_B$ 依然表示：“**如果选了 $SCC_A$ 里的点，就必须选 $SCC_B$ 里的点”**。

为了不引起矛盾，我们的原则是：尽量选“不强迫别人”的状态作为真值。

    拓扑序靠前（源点）：它指向很多后续节点，选它为“真”会触发一连串的“必须为真”。

    拓扑序靠后（汇点）：它不指向别人（或指向较少），选它为“真”非常安全。

结论： 在 $X$ 和 $¬X$ 两个状态中，谁的拓扑序更靠后（更接近汇点），谁就更有资格被赋值为“真”。


## 参考代码
```cpp
#include <climits>
#include <cstddef>
#include <queue>
#include <type_traits>
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


const int N = 2e5 + 10;
int n, m;
vector<int> adj[N*2];
int dfn[N], low[N], scc[N], sz[N], in_stack[N], sc, idf;
stack<int> st;

void record(){
    int v = st.top(); st.pop();
    scc[v] = sc;
    in_stack[v] = 0;
    sz[sc]++;
}

//正常tarjan
void tarjan(int u){
    dfn[u] = low[u] = ++idf;
    in_stack[u] = 1;
    st.push(u);
    for(auto v : adj[u]) {
        if(!dfn[v]) {
            tarjan(v);
            low[u] = min(low[u], low[v]);
        }else if(in_stack[v]) low[u] = min(low[u], dfn[v]);
    }

    if(dfn[u] == low[u]) {
        sc++;
        while(st.top() != u)
            record();
        record();
    }
}

signed main() {

    cin >> n >> m;

    auto get_node = [&](int x, char sign) {  //得到对应点
        x--;
        return (sign == '+') ? x<<1 :  x << 1 | 1;
    };

    auto neg = [&](int node) {return node ^ 1;};  //区负操作, 使用^小技巧

    fi(n) {
        int x, y;char s1, s2; cin >> s1 >> x >> s2 >> y;
        int u = get_node(x, s1);
        int v = get_node(y, s2);
        adj[neg(u)].push_back(v);  //建立新边
        adj[neg(v)].push_back(u);
    }

    for(int i = 0;i < m*2 ;++i) {  //注意建立的图含有2m个节点
        if(!dfn[i]) tarjan(i);
    }

    vector<char> ans(m+1);
    for(int i = 0;i < m; ++i){
        if(scc[i<<1]==scc[i<<1|1]) {
            cout << "IMPOSSIBLE\n";
            return 0;
        }
        ans[i] = (scc[i<<1] < scc[i<<1|1]) ? '+' : '-';  //尽量取不被依赖的
    }

    for(int i = 0;i < m; ++i) cout << ans[i] << ' ';

}

```
