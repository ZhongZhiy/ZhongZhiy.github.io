---
title: Hamster(二分图)
date: 2025-10-03 22:10:39
categories: [算法, 二分图]
tags: 补题
---

# 问题 F: Hamster
文件提交：无需freopen 内存限制：128 MB 时间限制：1.000 S
## 题目描述

The hamster lives in a rectangular enclosure with ﬂoor divided into square ﬁelds of the same size, forming a grid. In each ﬁeld, several larvae are dwelling. The hamster decided to walk around the enclosure, collect the larvae into his bowl, and bring them home for later culinary use. Additionally, he decided to place a trap for common starlings on each ﬁeld so that they would no longer ﬂy in and steal the larvae. The hamster starts in the northwest corner of the enclosure. From each visited ﬁeld, he collects the larvae and puts them in the bowl, places a trap on the ﬁeld, and moves to the next ﬁeld, which shares a side with the ﬁeld he is just leaving. 
The hamster will ﬁnish his journey in the southeast corner of the enclosure. But there is a catch,the hamster cannot return to ﬁelds he has already visited, otherwise he would get caught in one of his own traps.
We know how many larvae are in each ﬁeld. Find out the maximum number of larvae the hamster can collect on his journey and avoid all traps.
## 输入

First line contains two numbers N, M (2 ≤ N, M ≤ 1000), the number of rows and the number of columns in the grid of ﬁelds in the hamster’s enclosure. Then follow N lines, each contains M numbers representing numbers of larvae in particular ﬁelds in the enclosure. Each line represents
one row in the grid. The northwest corner of the grid corresponds to the ﬁrst number in the ﬁrst of these lines, the southeast corner of the grid corresponds to the last number in the last of these lines.
The number of larvae on any ﬁeld is between 0 and 1000 inclusive.
## 输出

Output a single number, the maximum number of larvae the hamster can collect.
### 样例输入

【样例1】
```
2 2
1 2
3 4
```
【样例2】
```
3 4
2 2 4 0
1 3 1 0
2 5 3 1
```
### 样例输出

【样例1】
```
8
```
【样例2】
```
24
```

## 题意
给定`n`行`m`列的非负数, 取从左上角, 到右下角的路径上的所有的数的和, 要求不能重复经过同一个位置两次, 求最大取值


## 题解
显然, 取得尽可能多的位置最好.
我们把%N \times M$ 的网格像国际象棋一样黑白相间地涂色：左上角（西北角）是白色，相邻格子颜色总是不同。
从一个格子走到相邻的上/下/左/右格子时，颜色一定会发生切换（白→黑或黑→白）。
整条路是一条不重复格子的“简单路径”。
简单路径有一个重要性质：

如果起点和终点颜色不同，路径访问的格子数是偶数。

如果起点和终点颜色相同，路径访问的格子数是奇数。
原因：每走一步颜色就切一次，奇偶性决定了两端颜色是否相同。

我们的起点在左上角（白），终点在右下角。右下角的颜色是否与左上角相同，取决于 N 和 M：

右下角坐标（用 1 开始计数）是 (N, M)，它与左上角 (1,1) 的“曼哈顿距离”是 (N−1)+(M−1)。这一步数若是偶数，两端同色；若是奇数，两端异色。

等价地说：当 N+M 为偶数 时，两端同色；当 N+M 为奇数 时，两端异色。
总共有 N×M 个格子。我们希望“吃到”尽可能多的幼虫；因为每个格子的幼虫数都是非负，理想情况当然是把所有格子都走到。

若 至少有一个维度是奇数（也就是 N 或 M 有一个是奇数），就能设计出“蛇形”路线把所有格子都走到并且刚好到达右下角。
这时答案就是全体格子幼虫数之和。

若 N 和 M 都是偶数，事情就变了：

这时 N×M 是偶数，而我们前面说过此时起终点颜色相同（因为 N+M 偶数），意味着任何不重复路径访问的格子数必须是奇数。

但“所有格子”是偶数个，这与“访问格子数必须是奇数”冲突——不可能走完所有格子。

最多只能少走一个格子。而为了能从“白色起点到白色终点”，路径中白格数量会比黑格数量多 1，所以被迫少走（跳过）的那个格子一定是黑格（坐标 i+j 为奇数的格子，1-based 下）。

幼虫都是非负的，当然要把损失最小化：跳过幼虫数最小的那个黑格。

于是答案 = 所有格子之和 −（所有黑格中的最小值）。

## 参考代码
```cpp

#include<bits/stdc++.h>
using namespace std;
#define caillo ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
#define PLEASE_AC return 0
typedef long long ll;
typedef unsigned long long ull;
#define int long long 

int n, m;
const int N = 1005;
int mp[N][N];


signed main() {
//	freopen("an", "r", stdin);
	caillo;

	cin >> n >> m;
	int min_b = INT_MAX, sum = 0;
	for(int i = 1;i <= n; ++i)
		for(int j = 1; j <= m; ++j){
			cin >> mp[i][j];
			sum += mp[i][j];
			if((i&1 && !(j&1)) || (!(i&1) && j&1)) min_b = min(min_b, mp[i][j]);
		}

	if(!(n&1) && !(m&1)) cout << sum - min_b;
	else cout << sum;

	PLEASE_AC;
}
```

