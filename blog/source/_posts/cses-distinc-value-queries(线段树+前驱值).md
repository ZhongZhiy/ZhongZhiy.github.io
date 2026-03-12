---
title: CSES Distinct Value Queries (线段树+前驱值)
date: 2026-01-19
categories: [算法, 线段树]
tags: CSES

---

# CSES Distinct Value Queries (线段树+前驱值)
## 题意
给定一个长度为 $n$ 的数组 $a$，支持以下操作：
1. 修改 $a[i]$ 的值为 $x$
2. 查询 $[l, r]$ 区间内是不是都是不同的

## 解法
问在 $[l, r]$ 区间内有没有相同的值, 可以转换为:
在 $[l, r]$ 区间内的值的左边的最近的相同值在不在这个区间中, 即:
`pre[a[i]]` 为`a[i]` 的左边的最近的相同值的下标, 只需要, `max{pre[a[i]]} < l` 即可。

## 参考代码
使用强制在线的写法, 查一点点超时
```cpp 
#include<bits/stdc++.h>
using namespace std;

#define DEBUG
#ifdef DEBUG
#define de(x) cout << (#x) << "=" << (x) << endl;
#define de2(x, y) cout << (#x) << " " << (#y) << " = " << (x) << " " << (y) << endl;
#else
#define de(x)
#define de2(x, y)
#endif
#define endl '\n'
#define fi(n) for(int i = 1;i <= n; ++i)
#define fi0(n) for(int i = 0;i < n; ++i)
#define fj(n) for(int j = 1;j <= n; ++j)
#define all(x) (x).begin(), (x).end()
#define hello ios::sync_with_stdio(0); cin.tie(0);cout.tie(0);
#define world return 0;
// #define int long long
typedef vector<int> vi;
typedef pair<int, int> pii;
typedef long long ll;

const int N = 2e5 + 10;
int tr[N<<2];
int a[N];
int lt[N], rt[N];

int n;
void pushup(int p) {
    tr[p] = max(tr[p<<1], tr[p<<1|1]);
}

void build(int p = 1, int l = 1, int r = n) {
    if(l > r) return;
    if(l == r) {
        tr[p] = lt[l];
        return;
    }

    int mid = (l + r) >> 1;
    build(p<<1, l, mid);
    build(p<<1|1, mid+1, r);
    pushup(p);
}

void update(int p, int l, int r, int pos) {
    if(l > r || pos < l || pos > r) return;
    if(l == r) {
        tr[p] = lt[l];
        return;
    }
    int mid = (l+r) >> 1;
    if(pos <= mid) update(p<<1, l, mid, pos);
    if(pos > mid) update(p<<1|1, mid+1, r, pos);
    pushup(p);
}

int query(int p, int l, int r, int cl, int cr) {
    if(l <= cl && r >= cr) return tr[p];
    int mid = (cl + cr) >> 1;
    int res = 0;
    if(l <= mid) res = max(res, query(p<<1, l, r, cl, mid));
    if(r > mid) res = max(res, query(p<<1|1, l, r, mid+1, cr));
    return res;
}

//记录每个每个数字出现的下表
map<int, set<int>> mst;
//修改, 先删除旧值, 再插入新值
void change(int x, int y) {
    if(a[x] == y) return;
    //删除旧值
    mst[a[x]].erase(x);
    //更新旧值附近的值
    a[x] = y;
    rt[lt[x]] = rt[x];
    lt[rt[x]] = lt[x];
    //更新线段树
    update(1,1, n, rt[x]);

    //查找新值插入附件的值
    auto pos = mst[y].upper_bound(x);
    //新值的下标是空的
    if(!mst[y].size()) {
        lt[x] = 0;
        rt[x] = n+1;
    } else {
        if(pos != mst[y].end()) {
            int p = *pos;
            rt[x] = p;
            lt[x] = lt[p];
            rt[lt[x]] = x;
            lt[p] = x;
        } else {
            pos--;
            int p = *pos;
            lt[x] = p;
            rt[x] = rt[p];
            lt[rt[x]] = x;
            rt[p] = x;
        }
    }
    mst[a[x]].insert(x);
    update(1, 1, n, x);
    update(1, 1, n, rt[x]);
}

void solve() {
    int q; cin >> n >> q;
    fi(n) cin >> a[i];
    //每个值修改
    fi(n) {
        if(!mst[a[i]].size()) {
            lt[i] = 0;
            rt[i] = n+1;
        }else {
            int b = *mst[a[i]].rbegin();
            lt[i] = b;
            rt[i] = n+1;
            rt[b] = i;
        }
        mst[a[i]].insert(i);
    }

    build();

    fi(q) {
        int op, x, y;cin >> op >> x >> y;
        if(op == 1) {
            change(x, y);
        }else {
            cout << (query(1,x, y, 1, n) >= x ? "NO\n" : "YES\n") ;
        }
    }
}

signed main() {
    hello;
    solve();
}

```
