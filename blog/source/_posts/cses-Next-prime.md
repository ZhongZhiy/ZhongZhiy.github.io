---
title: CSES - Next Prime
date: 2026-01-21
categories: [算法, 数论]
tags: CSES

---


## 题意
找大于这个数的第一给素数

## 题解
在$[n, 2n]$之间一定有至少一个素数, 所以直接判断
由于素数除了$2, 3$都在$6k\pm 1$上, 可以加速素数的判定
此外, 除了$2$以外的素数都是奇数, 那么如果这个数是奇数那么每次$\pm_{2}$, 偶数就先$+1$之后再处理

## 参考代码
```cpp fold
#pragma GCC optimize("Ofast")
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
#define int long long
typedef vector<int> vi;
typedef pair<int, int> pii;
typedef long long ll;

int n;
bool isprimer(int x) {
    if(x <= 1) return 0;
    if(x == 2 || x == 3) return 1;
    if(x % 2 == 0 || x % 3 == 0) return 0;

    for(int i = 5;i * i <= x; i += 6) {
        if(x % i == 0 || x % (i + 2) == 0) return 0;
    }
    return 1;
}



signed main() {
hello;
int t; cin >> t;
while(t--) {
    int n; cin >> n;
    //特别注意这两个数
    if(n == 1 || n == 2) {
        cout << (n == 1?2:3) << endl;
        continue;
    }
    if(!(n&1)) n++;else n+=2;
    while(!isprimer(n)) {n+=2;}
    cout << n << endl;
}

world;
}


```
