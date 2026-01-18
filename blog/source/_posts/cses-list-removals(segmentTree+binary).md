---
title: CSES - List Removals (Segment Tree + Binary Search)
date: 2026-01-15
categories: [算法, segment tree]
tags: CSES

---
# List Removals (Segment Tree + Binary Search)
## 题意
给定$n$个数, 每次查找第$i$个数字并删除
## 题解
使用线段树维护每个数字的存在性, 然后使用二分查找找到第$i$个数字并删除

## 参考代码
```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;
int mx[N << 2], a[N];
int n, m;

void pushup(int p) {
    mx[p] = (mx[p<<1]+ mx[p<<1|1]);
}

void build(int p = 1, int l = 1, int r = n){
    if(l == r){
        mx[p] = 1;
        return;
    }
    int mid = (l + r) >> 1;
    build(p<<1, l, mid);
    build(p<<1|1, mid+1, r);
    pushup(p);
}

int query_and_remove(int d, int p = 1, int l = 1, int r = n) {
    if(l == r){
        mx[p] = 0;return l;
    }
    int mid = (l + r) >> 1;
    int res = 0;
    if(mx[p<<1] >= d){
        res = query_and_remove(d, p<<1, l, mid);
    }else{
        //注意, 考虑右边就需要减去左边的个数
        res = query_and_remove(d-mx[p<<1], p<<1|1, mid+1, r);
    }
    pushup(p);
    return res;
}

signed main(){
    ios::sync_with_stdio(0);cin.tie(0);
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> a[i];
    build();
    for(int i = 1;i <= n; ++i){
        int x; cin >> x;cout << a[query_and_remove(x)] << ' ';
    }
}

```
