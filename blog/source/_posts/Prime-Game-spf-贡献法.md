---
title: Prime Game(spf, 贡献法)
date: 2025-10-06 21:24:56
categories: [算法, spf]
tags: 贡献法
---

#  问题 J: Prime Game

> 题目：给定序列 $(a_1,\dots,a_n)$（$(n\le 10^6,\ a_i\le 10^6)$）。
> 设 $( \text{mul}(l,r)=\prod_{i=l}^{r} a_i)，( \text{fac}(l,r))$ 为 $(\text{mul}(l,r))$ 的**不同质因子**个数。
> 求 $(\displaystyle \sum_{i=1}^{n}\sum_{j=i}^{n} \text{fac}(i,j))$。

---
#### 输入

The first line contains one integer n (1 ≤ n ≤ 106 ) — the length of the sequence.
The second line contains n integers ai (1 ≤ i ≤ n, 1 ≤ ai ≤ 106 ) — the sequence.

#### 输出

Print the answer to the equation.

#### 样例输入

```
10
99 62 10 47 53 9 83 33 15 24
```

#### 样例输出

```
248
```


## 题解

##  核心转化（贡献法 / 分解到质数）

对每个质数 (p) 单独计数它在多少个子数组中**出现过**（即区间里至少有一个元素被 (p) 整除）。
设 (C_p) 为这样的子数组个数，则
[
\sum_{l\le r}\text{fac}(l,r) ;=; \sum_{\text{质数 }p} C_p .
]
原因：每个子数组的“不同质因子个数”就是对**所有质数**做 0/1 贡献的求和；交换求和顺序即可（典型的贡献法）。

于是问题变为：**对每个出现过的质数 (p)**，统计“包含至少一个被 (p) 整除位置”的子数组数。

设所有长度为 (n) 的子数组总数为 (T=\frac{n(n+1)}{2})。
把数组中“被 (p) 整除的位置”作为障碍点，数组被这些点切成若干**不含 (p)** 的连续空段（gap）。
若某个 gap 的长度为 (g)，那么**完全落在这个 gap 内**的子数组都不含 (p)，共有 (\frac{g(g+1)}{2}) 个。
把所有 gap 的这类子数组数相加记为 (N_p)。则
[
C_p ;=; T - N_p .
]

> 小结：**只需求每个质数的“避开 (p) 的子数组个数”**，用总数减掉即可。

---

##  算法步骤（满足 (10^6) 规模）

1. **线性筛 / SPF（最小质因子）**到 (10^6)：支持 (O(\log a_i)) 分解每个 (a_i) 的**不同质因子**。

2. 扫描数组第 (i) 个元素，分解出其**不同**质因子集合 ({p})：

    - 对每个出现的 (p)，用 `last[p]` 记录上次出现 (p) 的位置（初始为 0，表示在 1 之前）。

    - 当前位置为 (i)，则**上一个出现位置到当前的 gap 长度**为 `gap = i - last[p] - 1`。把 (\frac{gap(gap+1)}{2}) 累加进 `noP[p]`（即 (N_p) 的部分和），然后更新 `last[p]=i`。

    - 第一次遇到某个 (p) 时，`gap = i-1`，正好统计左端头的空段。

    - 用一个 `visitedPrimes` 记录所有出现过的质数，便于收尾。

3. 扫描完所有位置后，对每个出现过的 (p) 再补上**尾端 gap**：
    `gap_tail = n - last[p]`，把 (\frac{gap_tail(gap_tail+1)}{2}) 加到 `noP[p]`。

4. 令 (T=\frac{n(n+1)}{2})。答案为
    [
    \sum_{p\ \text{出现过}} \bigl( T - \text{noP}[p] \bigr).
    ]

5. 全程使用 64 位整型保存答案与组合数量。


## 参考代码
```cpp

#include<bits/stdc++.h>
using namespace std;
#define i128 __int128
#define de(x) cout << (#x) << " = " << (x) << endl;
#define de2(x, y) cout << (#x) << " , " << (#y) << " = " << (x) << " ~ " << (y) << endl;
#define endl '\n'
#define fi(x) for(int i = 1; i <= x; ++i)
#define fi0(x) for(int i = 0; i < x; ++i)
#define fj(n) for(int j = 1; j <= n; ++j)
#define fj0(n) for(int j = 0; j < n; ++j)
#define caillo ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);
#define PLEASE_AC return 0
typedef long long ll;
typedef unsigned long long ull;
#define int long long

constexpr int N = 1e6 + 10;
int spf[N];
void pre_spf(){  //预处理, 用ai氏筛找spf
	for(int i = 2;i < N; ++i){
		if(!spf[i]){
			spf[i] = i;
			if(i * i > N) continue;
			for(int j = i * i; j < N; j += i){
				if(!spf[j]) spf[j] = i;
			}
		}
	}

	spf[1] = 1;
}

int last[N], seen[N], nop[N];  //last[p]是p上次出现的位置, seen[p]是记录p是不是第一次出现, nop[p]是记录不包含p的区间个数
signed main() {
//	freopen("an", "r", stdin);
	caillo;

	pre_spf();

	int n; cin >> n;

	vector<int> visited;  //记录所有出现的素数, 用于计算最后一段gap
	for(int i = 1;i <= n; ++i){
		int x; cin >> x;
		if(x == 1) continue;
		vector<int> primes;  //记录这数包含的素数
		while(x > 1){  //找素数
			int p = spf[x];
			primes.push_back(p);
			while(x % p == 0) x /= p;  //清除这个数的p因子
		}

		for(auto p : primes){  //计算从上一个位置到这位置的p的gap
			if(!seen[p]){seen[p] = 1;visited.push_back(p);}
			int gap = i - last[p] - 1;
			if(gap > 0) nop[p] += gap * (gap + 1) / 2;
			last[p] = i;
		}
	}

	for(auto p : visited){  //计算最后的gap
		int gap = n - last[p];
		if(gap > 0) nop[p] += gap * (gap + 1) / 2;
	}

	int t = n * (n + 1) >> 1;
	int ans = 0;
	for(auto p : visited) ans += (t - nop[p]);

	cout << ans << endl;

	PLEASE_AC;
}

```
