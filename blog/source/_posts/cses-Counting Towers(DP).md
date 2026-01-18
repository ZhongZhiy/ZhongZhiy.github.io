---
title: CSES - Counting Towers (DP)
date: 2026-01-16
categories: [算法, DP]
tags: CSES

---

# Counting Towers (DP)
## 题意
建造宽为$2$, 长为$n$的塔的堆砖方案数, 砖可以是任意长宽都为整数的.

## 题解
定义状态:$dp[i][1]$和$dp[i][0]$分别表示第$i$层是整体和两块砖的方案数.
状态转移: 
只关注第$i$层是怎么构建的: 
1. 如果第$i$层是一块整体的, 那么:
  1. 如果$i-1$层是一块整体, 它可以是和$i-1$层分开的, 也可以是和$i-1$层合并的
  2. 如果$i-1$层是两块砖, 那么$i$层只可以是和$i-1$层分开的
  3. 所以$dp[i][1] = 2 * dp[i-1][1] + dp[i-1][0]$
2. 如果第$i$层是两块砖, 那么:
  1. 如果$i-1$层是一块整体的, 那么第$i$层只能是和$i-1$层分开的
  2. 如果$i-1$层是两块砖, 那么考虑:
    1. 左合并, 右分开
    2. 右合并, 左分开
    3. 左分开, 右分开
    4. 左合并, 右合并
  1. 综上, $dp[i][0] = dp[i-1][1] + 4 * dp[i-1][0]$
  
  ## 参考代码
  ```cpp
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
#define all(x) (x.begin(), x.end())
#define hello ios::sync_with_stdio(0); cin.tie(0);cout.tie(0);
#define world return 0;
#define int long long
typedef vector<int> vi;
typedef pair<int, int> pii;
typedef long long ll;


const int N = 1e6 + 10;
const int M = 1e9 + 7;
int dp[N][2];
signed main() {
    int t; cin >> t;

    dp[1][0] = 1;dp[1][1] = 1;
    for(int i = 2; i < N; ++i) {
        dp[i][0] = (4 * dp[i-1][0] + dp[i-1][1]) % M;
        dp[i][1] = (2 * dp[i-1][1] + dp[i-1][0]) % M;
    }

    while(t--) {
        int n; cin >> n;
        cout << (dp[n][0] + dp[n][1]) % M << endl;

    }
}

```
