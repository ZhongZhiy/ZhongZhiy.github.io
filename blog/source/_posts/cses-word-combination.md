---
title: CSES Word Combination(字典树,DP)
date: 2026-01-08
categories: [算法, 字符串]
tags: trie

---
# CSES Word Combination(字典树,DP)
## 题意
给定一个字符串`s`, 给$n$个单词, 问:
这个字符串可以被这些单词组合成多少种方式?

## 题解
使用`dp`思想解决有多少种组合, 使用trie来快速匹配单词
1. 定义状态`dp[i]`表示前`i`个字符可以被这些单词组合成多少种方式
2. 初始化`dp[0] = 1`
3. 状态转移: 如果`dp[i] != 0`说明有以第i个字符结尾的单词, 那么就查找可以从i之后出发的单词在`i+1`到`j`之间, 那么`dp[j+1] = (dp[j+1] + dp[i]) % M`;
4. 返回`dp[s.size()]`

## code
```cpp

#pragma GCC optimize("Ofast")
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
// #define int long long


const int N = 1e6 + 10;
const int M = 1e9 + 7;
int node[N][26], idx;  //节点和节点编号
bool isEnd[N];

void insert(const string &s) {  //插入单词
    int p = 0;
    for(int i = 0; i < s.size(); ++i) {  //遍历单词的每个字符
        int ch = s[i] - 'a';
        if(!node[p][ch]) node[p][ch] = ++idx;   //如果当前节点没有这个字符，就新建一个节点
        p = node[p][ch];  //更新当前节点
    }
    isEnd[p] = true;  //标记单词结束
}

int dp[N];
signed main() {
    caillo;
    string s; cin >> s;
    int n; cin >> n;
    fi(n) {
        string t; cin >> t;
        insert(t);
    }

    dp[0] = 1;
    for(int i = 0;i < s.size(); ++i) {
        if(!dp[i]) continue;
        int p = 0;
        for(int j = i;j < s.size(); ++j) {
            int ch = s[j] - 'a';
            if(!node[p][ch]) break;

            p = node[p][ch];
            if(isEnd[p]) dp[j+1] = (dp[j+1] + dp[i]) % M;
        }
    }

    cout << dp[s.size()] << endl;
}

```
