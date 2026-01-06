---
title: CSES - Subtree Queries (重链剖分+线段树)
data: 2026-01-06
category: [算法, 重链剖分]
tags: [重链剖分]

---
# CSES - Subtree Queries (重链剖分+线段树)
## 题意
给定一棵树，每个节点有一个权值，支持两种操作：
1. 将节点 $x$ 的权值修改为 $d$。
2. 查询节点 $x$ 的子树中权值的和。

## 题解
使用重链剖分+线段树。
重链剖分可以把树剖成线性结构, 并且:
1. 每条重链的节点序列是连续的
2. 同一个子树上的节点序列也是连续的


这样就把树上问题转变为了单点修改和区间程序问题
使用线段树解决

## 参考代码
```cpp

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
typedef unsigned long long ull;
#define int long long  //不用longlong会越界

const int N = 2e5 + 10;
//fa记录每个节点的父节点, son记录重儿子, siz记录子树大小, idx为节点序列, top为重链的根节点, id为节点在序列中的位置, w记录节点的权值, new_w为节点转换为序列后的权值
int fa[N], son[N], dep[N], siz[N], dfs_ord[N], idx = 0, top[N], id[N], new_w[N], w[N];
vector<int> adj[N];

void dfs1(int x, int father) {  //初始化fa数组，dep数组，siz数组，son数组
    fa[x] = father;
    dep[x] = dep[father] + 1;
    siz[x] = 1;

    for(auto c : adj[x]) {
        if(c == father) continue;
        dfs1(c, x);
        siz[x] += siz[c];
        if(!son[x] || siz[son[x]] < siz[c]) {
            son[x] = c;
        }
    }
}

void dfs2(int x, int topx) {  //初始化top数组，id数组，new_w数组
    id[x] = ++idx;  //给每个节点一个序号
    new_w[idx] = w[x];  //序号对应节点的权值
    top[x] = topx;
    if(!son[x]) return;  //leaf 
    dfs2(son[x], topx);
    for(auto c : adj[x]) {
        if(c == fa[x] || c == son[x]) continue;

        dfs2(c, c);
    }
}

int tr[N<<2], tag[N<<2];  //线段树需要4倍空间

void pushup(int p) {
    tr[p] = tr[p<<1] + tr[p<<1|1];
}

void build(int p = 1, int l = 1, int r = idx) {
    if(l == r){
        tr[p] = new_w[l];
        return;
    }

    int mid = (l + r) >> 1;
    build(p<<1, l, mid);
    build(p<<1|1, mid+1, r);
    pushup(p);
}

inline void addtag(int p, int cl, int cr, int d) {
    tag[p] += d;
    tr[p] += (cr - cl + 1) * d;
}

void pushdown(int p, int l, int r) {
    if(tag[p]) {
        int mid = (l + r) >> 1;
        addtag(p<<1, l, mid, tag[p]);
        addtag(p<<1|1, mid+1, r, tag[p]);
        tag[p] = 0;
    }
}


void update(int l, int r, int d, int p = 1, int cl = 1, int cr = idx) {
    if(l <= cl && r >= cr) {
        addtag(p, cl, cr, d);
        return;
    }
    pushdown(p, cl, cr);
    int mid = (cl + cr) >> 1;  //注意此处为cl和cr

    if(l <= mid) update(l, r, d, p<<1, cl, mid);
    if(r > mid) update(l, r, d, p<<1|1, mid+1, cr);

    pushup(p);
}

int query(int l, int r, int p = 1, int cl = 1, int cr = idx) {
    if(l <= cl && r >= cr) {
        return tr[p];
    }
    pushdown(p, cl, cr);
    int mid = (cl + cr) >> 1;
    int sum = 0;
    if(l <= mid) sum += query(l, r, p<<1, cl, mid);
    if(r > mid) sum += query(l, r, p<<1|1, mid+1, cr);
    return sum;
}

int query_subtree(int x) {
    return query(id[x], id[x]+siz[x]-1);
}

void update_range(int x, int d){
    update(id[x], id[x], -query(id[x], id[x])+ d);
}

signed main() {
    caillo;
    int n, q; cin >> n >> q;
    fi(n) cin >> w[i];

    fi(n-1) {
        int x, y; cin >> x >> y;
        adj[x].push_back(y);
        adj[y].push_back(x);
    }

    //初始化操作
    dfs1(1, 0);
    dfs2(1, 1);
    build();

    fi(q) {
        int op; cin >> op;
        if(op == 2) {
            int x; cin >> x;
            cout << query_subtree(x) << endl;
        }else if(op ==1){
            int u, v; cin >> u >> v;
            update_range(u, v);
        }
    }
}

```
