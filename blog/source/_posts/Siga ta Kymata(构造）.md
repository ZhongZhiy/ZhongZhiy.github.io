---
title: Siga ta Kymata(构造）
data: 2025-11-26 0:25:00
categories: [算法, 构造]
tags: [构造, 补题]
---
# 题目： [B. Siga ta Kymata](https://codeforces.com/contest/2163/problem/B)

time limit per test: 1.5 seconds

memory limit per test: 256 megabytes

input: standard input

output: standard output

You are given a permutation$^{\text{∗}}$ $p$ of every integer from $1$ to $n$. You also own a binary$^{\text{†}}$ string $s$ of size $n$ where $s_i = \mathtt{0}$ for all $1 \le i \le n$. You may do the following operation at most $5$ times:

-   Choose any two integers $l$ and $r$ such that $1 \le l \le r \le n$. Then, for every $i$ such that $l < i < r$ and $\min(p_l, p_r) < p_i < \max(p_l, p_r)$ hold at the same time, you will set $s_i$ to $\mathtt{1}$.

You are also given a binary string $x$ of size $n$. After performing operations, it must hold for every $1 \le i \le n$ that if $x_i = \mathtt{1}$, then $s_i = \mathtt{1}$. Note that if $x_i = \mathtt{0}$, then $s_i$ can have **any** value.

Figure out any sequence of **at most $5$** operations such that the aforementioned condition is satisfied, or report that it is impossible to do so. Note that you **do not** have to minimize the number of operations you make.

$^{\text{∗}}$A permutation $p$ of every integer from $1$ to $n$ is a sequence of elements from $1$ to $n$ such that every element appears exactly once.

$^{\text{†}}$A string $b$ of size $m$ is considered binary if and only if $b_i = \mathtt{0}$ or $b_i = \mathtt{1}$ for all $1 \le i \le m$.
**Input**

Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^4$). The description of the test cases follows.

The first line of each test case contains a single integer $n$ ($3 \le n \le 2 \cdot 10^5$) — the size of the array.

The second line contains exactly $n$ integers $p_1, p_2, \ldots, p_n$ ($1 \le p_i \le n$, the elements of $p$ are pairwise distinct) — where $p_i$ is the $i$\-th element of the permutation.

The third line contains a single binary string $x$ of size $n$.

It is guaranteed that the sum of $n$ over all test cases does not exceed $2 \cdot 10^5$.

**Output**

For each test case, if it is impossible to perform operations such that the constraint is satisfied, output $-1$.

Otherwise, output an integer $0 \le k \le 5$, the number of operations. On the $i$\-th of the next $k$ lines, output two integers $1 \le l_i \le r_i \le n$, the bounds of the $i$\-th operation that is performed. If there are multiple correct solutions, output any of them.

**Note**

In the first example, $p = [1, 2, 3]$, and $x = \mathtt{010}$. We can perform a single operation, with $l = 1$ and $r = 3$. After the operation, we set $s_2$ to $\mathtt{1}$ since $l < 2 < r$ and $\min(p_l, p_r) < p_2 = 2 < \max(p_l, p_r)$ hold at the same time. As a result, $s = \mathtt{010}$.

In the second example, it can be shown that there does not exist a correct sequence of at most $5$ operations, so we output $-1$.

In the third example, $p = [1, 3, 2, 4, 6, 5]$ and $x = \mathtt{001100}$. After performing an operation for $l = 1$ and $r = 5$, then $s = \mathtt{011100}$. If we also perform an operation for $l = 2$ and $r = 6$, then $s$ will remain the same. The string $s = \mathtt{011100}$ is valid, because for every position where $x$ has a $\mathtt{1}$, then $s$ also has a $\mathtt{1}$ at that position.

## 题意
给定一个n的排列， 和01字符串s， 进行如下操作：
1. 选择$1 <= l <= r <= n$， 如果存在`a[l] < a[i] < a[r]`， 那么`s[i]`就可以为`1`， 否则不可以为一
2. 判断能不能形成字符串与s的`1`对应， `0`不用管

## 题解
考虑排列中首位x， 1所在的位， n所在的位， 末尾y
1. 对应x与1之间的数$k$, `1 <= k <= x`的数都对应1
2. 在x与n之间的所有数, `x <= k <= n`的数都对应1
3. 在x与n之间的所有数, `x <= k <= n`的数都对应1
4. 在x与n之间的所有数, `x <= k <= n`的数都对应1

**综上**： 
只有首位， 1所在的位， n所在的位， 末尾是不能变成1的

## 参考代码
```cpp

#include<bits/stdc++.h>
using namespace std;
#define i128 __int128

// #define DEBUG
#ifdef DEBUG
#define de(x) cout << (#x) << " = " << (x) << endl;
#define de2(x, y) cout << (#x) << " , " << (#y) << " = " << (x) << " ~ " << (y) << endl;
#else
#define de(x)
#define de2(x, y)
#endif
#define endl '\n'
#define fi(x) for(int i = 1; i <= x; ++i)
#define fi0(x) for(int i = 0; i < x; ++i)
#define fj(n) for(int j = 1; j <= n; ++j)
#define caillo ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
#define PLEASE_AC return 0
typedef long long ll;
typedef unsigned long long ull;
#define int long long

int n;
const int N = 2e5 + 10;

int a[N];
void solve(){
    cin >> n;
    int mx = -1, mn = -1;
    fi(n) {
        cin >> a[i];
        if(a[i] == 1) mn = i;
        if(a[i] == n) mx = i;
    }
    string s; cin >> s;
    if(s[0] == '1' || s[mn-1] == '1' || s[n-1] == '1' || s[mx-1] == '1'){
        cout << "-1\n";
        return;
    }else {
        set<pair<int, int>> ans;
        ans.insert({1, min(mx, mn)});
        ans.insert({1, max(mx, mn)});
        ans.insert({min(mx, mn), max(mx, mn)});
        ans.insert({max(mx, mn), n});
        ans.insert({min(mn, mx), n});
        cout << ans.size() << endl;
        for(auto [l, r] : ans) cout << l << ' ' << r << endl;
        return;
    }


}

signed main() {
    //	freopen("an", "r", stdin);
    caillo;

    int t; cin >> t;
    while(t--) solve();

    PLEASE_AC;
}

```
