---
title: Basic Graph Algorithm 24_shanghai
date: 2025-11-06 16:44:54
categories:[补题, dfs]
tags: [dfs]
---
# Basic Graph Algorithm
[2024年上海站区域赛B题](https://qoj.ac/contest/1913/problem/9038)
## 题意
使用BFS遍历图的时候, 有两点会导致遍历序列的不同:
1. 从最初没有visited的节点开始DFS, 这时候选择的开始节点可能不同
2. 从节点`u`遍历他的邻接节点的顺序可能不同
因此, 给定序列p, 求最小添加的边数, 使得DFS遍历能够有可能和给定p相同
输出最小添加的边的个数和新添加的对应边
## 题解
使用DFS遍历节点`u`的邻接节点时,
1. 如果存在target, 那么就调用`dfs(target)`
```cpp
for(auto v : e[u]){
    if(v == target) dfs(target);
}
```
2. 如果不存在对应的target, 那么就返回考虑需不需要添加新边
  1. 如果节点`u`还存在未访问的邻接节点, 那么一定需要添加新边, 否则DFS会走向其他边
  2. 如果节点`u`没有未访问的邻接节点, 那么就标记当前节点`ept[u] = 1`, 这样下次就不用再遍历了
    - 一般就直接返回到上一级
    - 但是如果当前就是开始DFS的节点, 那么就直接跳到target, 更新`start`也就相当于是从没visited中的节点开启一个新的DFS
    ```cpp
    if(!ept[u]){
        if(u == start) dfs(target);
    }
    ```
3. 最后考虑处理答案, 使用`exit(0)`省去一步步返回的麻烦
```cpp
void show(){
    cout << ans.size() << endl;
    for(auto [u, v] : ans){
        cout << u << ' ' << v << endl;
    }
    exit(0);
}
```
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
#define startime auto start = chrono::high_resolution_clock::now();
#define endtime auto end = chrono::high_resolution_clock::now();
#define runningtime cout << "running time: " << chrono::duration_cast<chrono::milliseconds>(end - start).count()<<" ms\n";
typedef long long ll;
typedef unsigned long long ull;
#define int long long
int n, m;
constexpr int N = 3e5 + 10;
bool vis[N], ept[N];
vector<int> e[N];
vector<pair<int, int>> ans;
int order[N];
int idx = 2, start = 1;
void show(){
	cout << ans.size() << endl;
	for(auto [u, v] : ans) cout << u << ' ' << v << endl;
	exit(0);
}
void dfs(int u){
	vis[u] = 1;
	if(idx == n + 1){
		show();
	}
	if(ept[u] && u == order[start]){  //if this is empty node and start node
		start = idx;
		dfs(order[idx++]);
	}else if(ept[u]){  //if this is empty node, just return;
		return;
	}
	int no_ex = 0;
	while(!ept[u]){  //if there exist any node
		no_ex = 0;
		int found = 0;
		for(auto v : e[u]){
			if(!vis[v]) no_ex = 1;  //exist not visited node

			if(v == order[idx]){  //if find the target
				found = 1;
				dfs(order[idx++]);
			}
		}
		if(!no_ex) ept[u] = 1;  //if no node
		if(!found){  //not find the node
			if(ept[u]) {  //all adjnode be visited
				if(u == order[start]){
				start = idx;
				dfs(order[idx++]);
				}
			}else {  //exist node not visited
				ans.push_back({u, order[idx]});
				dfs(order[idx++]);
			}
		}
	}
}
signed main() {
	// freopen("an", "r", stdin);
	caillo;
	cin >> n >> m;
	fi(m){
		int x, y; cin >> x >> y;
		e[x].push_back(y);
		e[y].push_back(x);
	}
	fi(n) cin >> order[i];
	dfs(order[start]);
	PLEASE_AC;
}
```
