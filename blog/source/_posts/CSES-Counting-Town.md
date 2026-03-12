---
title: CSES Counting Towns
date: 2026-01-25
categories:
tags:
  - DP
  - 算法
status:
difficulty: 🟡 中等
link: https://cses.fi/problemset/task/2413
---

# CSES Counting Towns
## 题意

> [!abstract] 题目关键信息
> 问$2\times n$的楼有几种搭建方案, 使用$x \times y$的矩形砖块搭建


##  题解
每次考虑最顶上的一层, 例如第$i$层
1. 如果第$i$层是一个联通的砖块, 那么考虑$i-1$层的情况
	1. $i-1$层也是一个联通的砖块, 那么$i$可以有两种情况: 单独的$1 \times 2$砖块, 或者是和$i-1$层连在一起
	2. $i-1$是分离的两块砖, 那么对于$i$层, 就是能是一个$1\times {2}$的砖块了
2. 如果$i$是两块分离的砖块, 考虑$i-1$层的情况: 
	1. 如果第$i-1$层是一个整块, 那么就只有一种情况
	2. 如果第$i-1$是分离的两块, 那么有$4$种情况需要讨论:
		1. 左左相连, 右右相连
		2. 左左相连, 右右相离
		3. 左左相离, 右右相连
		4. 左右都相离

**定义状态**: `dp[i][0]`为第$i$层是联通的, `dp[i][1]`是分离的
**状态转移**: `dp[i][0] = dp[i-1][0] + 2 * dp[i-1][1]`
`dp[i][1] = dp[i-1][1] + 4 * dp[i-1][0]`
**基本情况**: 只有一层的情况`dp[1][0] = dp[1][1] = 1`



##  参考代码
> [!example] 
```cpp
// 只需要记录最核心的代码块或模板
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
typedef vector<vector<int>> vvi;
typedef long long ll;
const int N = 1e6 + 10;
const int M = 1e9 + 7;
int dp[N][3];
signed main() {
    hello;
    dp[1][0] = 1; dp[1][1] = 1;
    for(int i = 2;i < N; ++i) {
        dp[i][0] = (2 * dp[i-1][0] + dp[i-1][1]) % M;
        dp[i][1] = (4 * dp[i-1][1] + dp[i-1][0]) % M;
    }
    int t; cin >> t;
    while(t--) {
        int n; cin >> n;
        cout << (dp[n][0] + dp[n][1]) % M << endl;
    }
    world;
}
```
