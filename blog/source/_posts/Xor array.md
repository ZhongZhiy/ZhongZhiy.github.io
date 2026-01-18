---
title: Xor Array(xor构造）
date: 2025-12-12
categories: [算法，xor]
tags : xor

---
# [xor array](https://codeforces.com/contest/2175/problem/B)
## 题意
构建含有n个数的数组满足以下两个条件：
1. 在l和r之间的所有数异或和为0
2. 除了l和r的区间以外的子段数异或和都不为零
求这数组

## 题解
异或和应该多考虑前缀异或和，
$$
b_i = a_1 \oplus a_2 \oplus a_3 \oplus \cdots \oplus a_i
$$，
那么l和r之间的子段异或和为0，可以表述为：
$$
b_{l-1} \oplus b_r = 0
$$
此外所有异或和不为零，我们可以给他们赋值为$b_i = i$, 让$b_{l-1} = b_r = 0$，这样$l$和$r$之间的数异或和为0，其他数异或和都不为零。

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
#define int long long

const int N = 2e5 + 10;


void solve(){
    int n,l, r; cin >> n >> l >> r;
    vector<int> b(n + 1);
    int idx = 1;
    for(int i = 1;i <= n ;++i){
        if(i == r) b[i] = b[l-1];
        else b[i] = i;
    }
    vector<int> a(n + 1);

    fi(n){
        a[i] = b[i] ^ b[i-1];
    }
    fi(n) cout << a[i] << ' ';
    cout << enld;
}

signed main() {
    //	freopen("an", "r", stdin);
    caillo;

    int t;
    cin >> t;
    while(t--) solve();

    PLEASE_AC;
}

```
