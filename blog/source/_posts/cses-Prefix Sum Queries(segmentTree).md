---
title: CSES Prefix Sum Queries(segmentTree)
date: 2026-01-15
categories: [算法, 线段树]
tags: CSES

---

# CSES Prefix Sum Queries(segmentTree)
## 题意
给定一个长度为 $n$ 的数组 $a$，支持以下操作：
1. 修改 $a[i]$ 的值为 $x$
2. 查询$[a,b]$ 的前缀和中的最大值

## 题解
使用线段树维护区间和和区间最大前缀和
如果一个区间分成了两部分, 例如$[1, 6]$分成了$[1,3], [4,6]$，那么最大前缀和就是$[1,3]$的最大前缀和和$[4,6]$的最大前缀和$+[1,3]$的区间和 中的最大值
$$maxf[p] = max(0LL, max(l.maxf, l.sum + r.maxf))$$

因为最大前缀和是一个**有序且相互依赖的**属性, 不可以简单拼凑

## 参考代码
```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long
const int N = 2e5 + 10;

//每个节点都需要两个信息
struct Node {
    int sum, maxf;
};
Node tr[N<<2];

int a[N];
int n, m;
Node merge(Node l, Node r){  //merge合并子树的信息
    return {l.sum + r.sum, max(0LL, max(l.maxf, l.sum + r.maxf))};
}

void build(int p = 1, int l = 1, int r = n){
    if(l == r){
        tr[p].sum = a[l];
        tr[p].maxf = max(0LL, a[l]);  //注意可能空前缀的时候反而最大
        return;
    }

    int mid = (l + r) >> 1;
    build(p<<1, l, mid);
    build(p<<1|1, mid+1, r);
    tr[p] = merge(tr[p<<1], tr[p<<1|1]);
}

void update(int l, int r, int d, int p  = 1,int cl = 1 ,int cr = n) {
    if(l <= cl && r >= cr) {
        tr[p].sum = d;
        tr[p].maxf = max(0LL, d);
        return;
    }
    int mid = (cl + cr) >> 1;
    if(l <= mid) update(l, r, d, p<<1, cl, mid);
    if(r > mid) update(l, r, d, p<<1|1, mid+1, cr);
    tr[p] = merge(tr[p<<1], tr[p<<1|1]);
}

Node query(int l, int r, int p = 1, int cl = 1, int cr = n) {
    if(l <= cl && r >= cr) {
        return tr[p];
    }
    int mid = (cl + cr) >> 1;
    //这里逻辑和一般线段树稍有不同, 只访问有的部分
    if(r <= mid) return query(l, r, p<<1, cl, mid);
    if(l > mid) return query(l, r, p<<1|1, mid+1, cr);
    //这里合并信息
    return merge(query(l, r, p<<1, cl, mid), query(l, r, p<<1|1, mid+1, cr));
}

signed main() {
    ios::sync_with_stdio(0);cin.tie(0);
    cin >> n >> m;
    for(int i = 1;i <= n; ++i) {
        cin >> a[i];
    }
    build();

    for(int i = 1;i <= m; ++i) {
        int o, x, y; cin >> o >> x >> y;
        if(o == 1) {
            update(x, x, y);
        }else {
            cout << query(x, y).maxf << endl;
        }
    }
}

```
