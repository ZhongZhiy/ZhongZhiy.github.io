---
title: CSES Distinct Value Queries (可持久化线段树)
date: 2026-01-18
categories: [算法, 可持久化线段树]
tags: CSES

---
# CSES Distinct Value Queries (可持久化线段树)
## 题意
给定一个长度为 $n$ 的数组 $a$，支持两种操作：
1. 询问区间 $[x, y]$ 中不同元素的个数。

## 解法
使用可持久化线段树维护区间内不同元素的个数。
在数字最后一次出现的位置记录1, 之前出现的位置记录0
因为重复数字只贡献最后一次

## 参考代码
```cpp
#include <algorithm>
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
struct Node {
    int l, r, val;
    Node(){}
    Node(int l, int r, int val):l(l), r(r), val(val){}
};

Node tr[N*40];  //开大了超内存
int a[N], root[N], idx;

//插入, 更新节点个数
int update(int pre, int x, int d, int l, int r) {
    int cur = ++idx;
    tr[cur] = tr[pre];
    if(l == r) {
        tr[cur].val += d;
        return cur;
    }
    int mid = (l + r) >> 1;
    if(x <= mid) tr[cur].l = update(tr[pre].l, x, d, l, mid);
    else tr[cur].r = update(tr[pre].r, x, d, mid+1, r);
    tr[cur].val = tr[tr[cur].l].val + tr[tr[cur].r].val;
    return cur;
}

//在范围l, r中, 版本p的树上有多少个不同的数
int query(int l, int r, int p, int cl, int cr) {
    if(l <= cl && r >= cr) {
        return tr[p].val;
    }
    int mid = (cl + cr) >> 1;
    int res = 0;
    if(l <= mid) res += query(l, r, tr[p].l, cl, mid);
    if(r > mid) res += query(l, r, tr[p].r, mid+1, cr);
    return res;
}

signed main() {
    hello;
    int n, m; cin >> n>> m;
    fi(n) {
        cin >> a[i];
    }

    //记录数字a[i]最后一次出现的位置
    map<int, int> mp;
    fi(n) {
        //对于存在的数, 先建立一个去掉它的版本, 在建一个新版本存在
        if(mp.count(a[i])) {
            int tp = update(root[i-1], mp[a[i]], -1, 1, n);
            root[i] = update(tp, i, 1, 1, n);
        }else {
            root[i] = update(root[i-1], i, 1, 1, n);
        }
        mp[a[i]] = i;
    }

    fi(m) {
        int x, y; cin >> x >> y;
        cout << query(x, y, root[y], 1, n) << endl;
    }
    world;

}

```
