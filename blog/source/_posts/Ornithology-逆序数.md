---
title: Ornithology(逆序数)
date: 2025-10-03 19:38:59
categories: [算法, 逆序数]
tags: [逆序数]
---

# 问题 H: Ornithology
文件提交：无需freopen 内存限制：128 MB 时间限制：1.000 S
## 题目描述

On the outskirts of the town, close to the farm, there stand two parallel power lines, separated by a narrow dirt road. The power lines were a favorite resting spot for a variety of birds.
Today, on this cool autumn morning, the lines are ﬁlled with a group of birds. On the ﬁrst line there are n consecutive positions for birds numbered 0, 1, . . . , n − 1. On the second power line there are also n positions numbered in the same manner.
Initially, every position of the ﬁrst line is occupied by some birds (possibly zero) and there are no birds on the second power line. Each bird has its desired position to ﬂy to on the second line. No two birds on the same position on the ﬁrst line share the same desired position.
At one moment, all the birds at once will decide to ﬂy to their desired positions. Every bird ﬂies along the straight line segment connecting its initial and desired position.
It can happen that some pairs of birds crash into each other during their ﬂight. This can happen when their corresponding line segments cross. We shall call such unordered pair of birds dangerous pair.
For example, if a bird on position 2 wants to go to position 1 and the bird on position 1 wants to go to position 2, their paths cross.
The birds will not collide if their paths have the same desired position (on the second line) or if they start from the same position (on the ﬁrst line). In other words a pair of birds with the same initial or desired positions is not considered a dangerous pair.
The task is very simple. Compute the number of dangerous pairs of birds.
## 输入

First line of input contains an integer n (1 ≤ n ≤ 2 · 105 ) representing the number of positions on both power lines.
Each of the next n lines describe the desired positions of the birds. The i-th line starts with an integer pi (0 ≤ pi ≤ n) representing the number of birds on position i.
Then, there are pi distinct numbers qi,1 , . . . , qi,pi (0 ≤ qi,j≤ n−1) representing the desired places of the pi birds.
It is guaranteed that the does not exceed 2 · 105 .
## 输出

Output exactly one line containing one integer – the number of dangerous pairs of birds.
### 样例输入
```
3
2 1 2
1 0
1 1
```
### 样例输出
```
3
```

## 题意
有两根平行的线, 一根线上有n给位置, 每个位置可能和对面某个位置连线, 一个位置可以和多个位置连线, 也可以没有连线, 问: 这些连线有多少个交点

## 题解思路
考虑到如果一条线起点`a`, 终点`b`, 那么起点大于`a`的点, 终点小于`b`就一定会和`ab`这条线相交, 那么我们按照起点排序, 然后所有的终点构成的序列的逆序数就是答案.
对于求逆序数, 使用树状数组维护每个数的频数, 每个数字的逆序数就是从后往前枚举每个数之前的频数之和
例如样例的z数列终点逆序数:
```
终点   1 2 0 1
逆序数 0 0 2 1
```
所以相加为3
## 参考代码
```cpp

#include<bits/stdc++.h>
using namespace std;
#define caillo ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
#define PLEASE_AC return 0
typedef long long ll;
typedef unsigned long long ull;
#define int long long

const int N = 2e5 + 10;
int n, idx = 0;
int a[N], tr[N];

int lowbit(int x){return x & -x;}

void add(int x){
	for(int pos = x; pos <= n; pos += lowbit(pos))
		tr[pos]++;
}

int query(int x) {
	int sum = 0;
	for(int pos = x - 1; pos; pos -= lowbit(pos))
		sum += tr[pos];
	return sum;
}

signed main() {
	// freopen("an", "r", stdin);
	caillo;

	cin >> n;
	for(int i = 1;i <= n; ++i) {
		int t; cin >> t;
		for(int j = 1;j <= t; ++j) {
			++idx; cin >> a[idx];
			a[idx] += 2;  //由于需要查询前一个, 而且有零, 所以把所有的数有意2位, 这样就可以正常使用树状数组了
		}
		sort(a + 1 + idx - t, a + 1 + idx);  //对于每个起点的连线, 需要确保终点也是有序的
	}
	int ans = 0;
	for(int i = idx; i ; --i){
		ans += query(a[i]);
		add(a[i]);
	}
	cout << ans;

	PLEASE_AC;
}
```
