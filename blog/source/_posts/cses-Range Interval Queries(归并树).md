---
title: CSES Range Interval Queries(可持久化线段树/归并树)
date: 2026-01-16
categories: [算法, 归并树]
tags: CSES

---
# CSES Range Interval Queries
## 题意
求在区间$[l,r]$内大小为$[c, d]$的数的个数。

## 题解
使用归并树, 暴力求解
归并树就是线段树的节点换成一个有序向量, 这样, 通过在线段树内找到范围$[l, r]$, 然后直接二分找在$[c, d]$内的数的个数。


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
#define de(x) 42
#define de2(x, y) 42
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
vi tr[N<<2];
int a[N];

int n, m;
void build(int p = 1, int l = 1, int r = n) {
    if(l == r) {
        tr[p].push_back(a[l]);
        return;
    }
    int mid = (l + r) >> 1;
    build(p<<1, l, mid);
    build(p<<1|1, mid+1, r);
    //分配空间
    tr[p].resize(r - l + 1);
    //合并
    merge(all(tr[p<<1]), all(tr[p<<1|1]), tr[p].begin());
}

//计算节点p的范围内
int cnt(int p, int c, int d) {
    auto l = lower_bound(all(tr[p]), c);
    auto r = upper_bound(all(tr[p]), d);
    return distance(l, r);
}

int query(int l, int r, int c, int d, int p = 1, int cl = 1, int cr = n) {
    if(l <= cl && r >= cr) {
        return cnt(p, c, d);
    }
    int mid = (cl + cr) >> 1;
    int ans = 0;
    if(l <= mid) ans+= query(l, r, c, d, p<<1, cl, mid);
    if(r > mid) ans+= query(l, r, c, d, p<<1|1, mid+1, cr);
    return ans;
}

signed main() {
    hello;
    cin >> n >> m;
    fi(n) cin >> a[i];
    build();

    fi(m){
        int a, b, c, d; cin >> a >>  b>> c >> d;
        cout << query(a, b, c, d) << endl;
    }
    world;
}

```

## 可持久化线段树解法
可以把范围$[a, b]$当作是版本, 这样就可以解读成, 在插入版本$a$和插入版本$b$之间$[c, d]$之间的个数有多少个, 而求$[c, d]$之间的个数就是简单线段树的问题了


## 题解
```cpp
#include <algorithm>
#include<bits/stdc++.h>
using namespace std;

#define DEBUG
#ifdef DEBUG
#define de(x) cout << (#x) << "=" << (x) << endl;
#define de2(x, y) cout << (#x) << " " << (#y) << " = " << (x) << " " << (y) << endl;
#else
#define de(x) 42
#define de2(x, y) 42
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
int a[N], b[N];
//分别指向左右子树节点, 和当前节点范围内的节点个数
struct Node{
    int l, r, cnt;
};
Node tr[N<<5];
int idx = 0;
int root[N];  //版本标号

//更新, 就是新建一个根节点, 向下建树, 如果插入的点在左子树, 就向左子树建点
int update(int pre, int v, int l, int r) {
    //分配节点
    int cur = ++idx;
    //复制前一个版本
    tr[cur] = tr[pre];
    tr[cur].cnt++;
    //处理叶子节点
    if(l == r) return cur;
    int mid = (l + r) >> 1;
    if(v <= mid) tr[cur].l = update(tr[pre].l, v, l, mid);
    else tr[cur].r = update(tr[pre].r, v, mid+1, r);
    //返回根节点
    return cur;
}

//在版本u+1到版本v之间, 范围在l和r之间的点的个数, ql和qr是查询节点
int query(int u, int v, int l, int r, int ql, int qr) {
    if(r < l) return 0;
    if(l <= ql && r >= qr) {
        return tr[v].cnt - tr[u].cnt;
    }

    int mid = (ql + qr) >> 1;

    int res = 0;
    if(l <= mid) res += query(tr[u].l, tr[v].l, l, r, ql, mid);
    if(r > mid) res += query(tr[u].r, tr[v].r, l, r, mid+1, qr);
    return res;
}

signed main() {
    hello;

    int n, m; cin >> n >> m;
    fi(n) {
        cin >> a[i];
        b[i] = a[i];
    }

    //离散化
    sort(a+1, a+1+n);
    int sz = unique(a+1, a+1+n) - (a+1);

    for(int i = 1;i <= n; ++i ) {
        int ri = lower_bound(a+1, a+1+sz, b[i]) - a;
        root[i] = update(root[i-1], ri,  1, sz);
    }


    fi(m) {
      int u, v, l, r; cin >> u >> v >> l >> r;
      int rl = lower_bound(a+1, a+1+sz, l) - a ;
      int rr = upper_bound(a+1, a+1+sz, r) - (a+1);
      cout << query(root[u-1], root[v], rl, rr, 1, sz) << endl;
    }


}
```
