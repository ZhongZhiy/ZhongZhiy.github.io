---
title: bound found(尺取法)
date: 2025-09-23 22:03:43
categories: [算法, 尺取法]
tags: [滑动窗口]
---

[题干](https://vjudge.net/problem/POJ-2566#author=0)
## 题意
找数列中最接近给定值的子串和

## 思路
直接对原数列使用滑动窗口不行, 因为没有单调性
但是可以考虑到, 子串和就相当于前缀和的差, 可以用前缀和排序, 这样就有单调性了, 每次滑动窗口记录`sum = head[r] - head[l]`的值, 当`sum < t`就`r++`, 当`sum > t`就`l++`, 这样不断逼近, 每次记录原始的下标, 但是值得注意的是更新`l`和`r`的时候会产生`l >= r`, 需要特别处理

```cpp


#include <iostream>
#include<algorithm>
#include<climits>
using namespace std;

typedef long long ll;
struct Node {
    ll sum;
    ll id;    //原始下标
    Node(){}
    Node(ll s, ll i): sum(s), id(i) {}
    bool operator < (const Node& o) const {
        return sum < o.sum;
    }
};

ll abs(ll aa){return aa < 0 ? -aa : aa;}

const ll MAXN = 100000 + 10;
Node head[MAXN];
ll a[MAXN];
ll n, m;

void solve() {
    head[0].sum = 0;
    head[0].id = 0;
    for (ll i = 1; i <= n; ++i) {
        cin >> a[i];
        head[i].sum = head[i-1].sum + (ll)a[i];
        head[i].id = i;
    }

    sort(head, head + n + 1);   //需要第0个前缀和

    for (ll qi = 0; qi < m; ++qi) {
        ll t; cin >> t;

        ll l = 0, r = 1;
        ll bestDiff = LLONG_MAX;
        ll bestSum = 0;      // the absolute sum value to output
        ll bestL = 1, bestR = 1;

        while (l <= n && r <= n) {
            if (l == r) { ++r; continue; }

            // Because head is sorted by prefix sum, head[r].sum >= head[l].sum,
            // so sum = head[r].sum - head[l].sum >= 0 and equals |P[id_r] - P[id_l]|.
            ll sum = head[r].sum - head[l].sum;
            ll diff = (sum >= t) ? (sum - t) : (t - sum); // |sum - t|

            if (diff < bestDiff || (diff == bestDiff && sum < bestSum)) {
                bestDiff = diff;
                bestSum = sum;
                ll id1 = head[l].id;
                ll id2 = head[r].id;
                bestL = min(id1, id2) + 1; // subarray starts at min(id)+1
                bestR = max(id1, id2);     // subarray ends at max(id)
            }

            if (sum > t) ++l;
            else ++r;
        }

        cout << bestSum << ' ' << bestL << ' ' << bestR << '\n';
    }
}

int main() {
    ios::sync_with_stdio(false);

    while ( (cin >> n >> m) && (n || m) ) {
        solve();
    }
    return 0;
}

```
