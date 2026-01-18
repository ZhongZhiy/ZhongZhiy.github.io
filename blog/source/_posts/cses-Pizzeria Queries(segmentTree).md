---
title: CSES - Pizzeria Queries(segmentTree)
date: 2026-01-15
categories: [算法, 线段树]
tags: CSES

---
# CSES - Pizzeria Queries(segmentTree)
## 题意
给定$n$个数, 求: 
1. 把第$k$个数修改为$d$
2. 对于从$a$到$k$, 有值$|a-k| + p[a]$, 求对于$k$的最小值

## 题解
对公式变形: 
1. $k >= a$时, $|a-k| + p[a] = (p[a] - a) + k$
2. $k < a$时, $|a-k| + p[a] = (p[a] + a) - k$
其中$k$对于点$k$是定值, 也就是只需要求出$min(p[a] - a), min(p[a] + a)$就行了

## 代码实现
```cpp
#include <bits/stdc++.h>
#include <climits>
using namespace std;

#define int long long
const int N = 2e5 + 10;

//每个节点都需要两个信息
struct Node {
    int dec, add;
    Node(){}
    Node(int dec, int add):dec(dec), add(add){}
};

Node tr[N<<2];
int a[N];
int n, m;

void pushup(int p){
    tr[p].dec = min(tr[p<<1].dec, tr[p<<1|1].dec);
    tr[p].add = min(tr[p<<1].add, tr[p<<1|1].add);
}

void build(int p = 1, int l = 1, int r = n){
    if(l == r) {
        tr[p] = {a[l]-l, a[l]+l};
        return;
    }
    int mid = (l + r) >> 1;
    build(p<<1, l, mid);
    build(p<<1|1, mid+1,r);
    pushup(p);
}

void update(int l, int r, int d, int p = 1, int cl = 1, int cr = n) {
    if(l <= cl && r >= cr) {
        tr[p].dec = d-l;
        tr[p].add = d+l;
        a[l] = d;
        return;
    }
    int mid = (cl + cr) >> 1;
    if(r <= mid) update(l, r, d, p<<1, cl, mid);
    if(l > mid) update(l, r, d, p<<1|1, mid+1, cr);
    pushup(p);
}

int query(int l, int r,int sg,  int p = 1, int cl = 1, int cr = n) {
    if(l > r || l > n) return INT_MAX;  //注意要剪枝
    if(l <= cl && r >= cr) {
        return sg == 1 ? tr[p].dec : tr[p].add;
    }
    int mid = (cl + cr) >> 1;
    if(r <= mid) return query(l, r, sg, p<<1, cl, mid);
    if(l > mid) return query(l, r, sg, p<<1|1, mid+1, cr);
    return min(query(l, r, sg, p<<1, cl, mid), query(l, r, sg, p<<1|1, mid+1, cr));
}

int query_min(int k) {
    return min({query(1,k-1,1)+k, query(k+1,n,0)-k, a[k]});
}

signed main() {
    ios::sync_with_stdio(0);cin.tie(0);
    cin >> n >> m;
    for(int i = 1;i <= n; ++i){
        cin >> a[i];
    }
    build();
    for(int i = 1;i <= m; ++i) {
        int o; cin >> o;
        if(o == 2){
            int k; cin >> k;
            cout << query_min(k) << endl;
        }else {
            int k, u; cin >> k >> u;
            update(k,k,u);
        }
    }
}

```
