---
title: Call of Accepted
date: 2025-09-28 09:38:57
categories: [算法, 栈]
tags: [中缀表达式]
---

## 问题 B: Call of Accepted
时间限制: 1.000 Sec  内存限制: 128 MB
## 题目描述
You and your friends are at the table, playing an old and interesting game - the Call of Cthulhu.
There is a mechanism in the game: rolling the dice. You use a notation to communicate the type of dice that needs to be rolled - the operator "d". "x d y " means that an y-sided dice should be rolled x times and the sum of the results is taken.
Formally, for any two integers  x,y satisfying  x≥0 and  y≥1 , "x d y  " means the sum of  x random integers in the range of  [1,y]. It's obvious that either x<0 or y<1  makes the expression x d y  illegal. For example: "2 d 6" means that rolling a 6-sided dice 2 times. At this time, the result can be at least [1,1]=2 , and at most [6,6]=12 . The results of rolling can be used extensively in many aspects of the game. For example,an "1 d 100 " can be used to determine whether an action with a percentage probability is successful, and a "3 d 6 + 3 * 2" can be used to calculate the total damage when being attacked by 3 monsters simultaneously. 
In particular, the precedence of "d" is above "*". Since the operator "d" does not satisfy the associative law, it's necessary to make sure that "d" is right-associative.
Now the game has reached its most exciting stage. You are facing the Great Old One - Cthulhu. Because the spirit has been greatly affected, your sanity value needs to be deducted according to the result of rolling. If the sanity value loses too much, you will fall into madness. As a player, you want to write a program for knowing the minimum and maximum loss of sanity value before rolling, in order to make a psychological preparation.
The oldest and strongest emotion of mankind is fear, and the oldest and strongest kind of fear is fear of the unknown. ----H. P. Lovecraft
### 输入
There are multiple sets of input, at most 30 cases.
Each set of input contains a string of only "+", "-", "*", "d", "(", ")" and integers without spaces, indicating the expression of this sanity loss. The length of the expression will be at most 100.
It is guaranteed that all expressions are legal, all operators except for '(' and ')' are binary, and all intermediate results while calculating are integers in the range of  [-2147483648,2147483647].
The most merciful thing in the world, I think, is the inability of the human mind to correlate all its contents. We live on a placid island of ignorance in the midst of black seas of inﬁnity, and it was not meant that we should voyage far. ----H. P. Lovecraft
### 输出
For each set of data, output a line of two integers separated by spaces, indicating the minimum and maximum possible values of the sanity loss.	
#### 样例输入 Copy

3d6*5
2d3d4

#### 样例输出 Copy

15 90
2 24


## 题意
存在一个符号`d`, `adb`代表`a`面骰子投掷`b`次, 那么取值范围为\[a, a*b\], 且`d`只满足右结合, 求一个表达式的最大值和最小值

## 题解
这道题是中缀表达式转后缀表达式, 使用一个符号栈, 和数值栈模拟, 但是值得注意的是:
1. 常规符号+, -, *都是左结合, 而d是右结合的, 因此, 遇到同级优先级或是更高的在符号栈顶, 对于常规符号需要计算栈顶符号, 但是对于d不需要
2. 需要同时计算最小值到最大值, 每个符号需要仔细考虑
3. 读取原始表达式, 仔细考虑数字是否有一元符号, 例如-2

### 参考代码
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

string s;

stack<pair<int, int> > num;
stack<char> ch;


void cal(){
	auto [a, b] = num.top(); num.pop();
	auto [c, d] = num.top(); num.pop();// [c d]-[a b]
	auto chr = ch.top(); ch.pop();
	int mx = 0, mn = 0;
	if(chr == '+') mn = a + c, mx = b + d;
	else if(chr == '-') mn = c - b, mx = d - a;
	else if(chr == '*') mn = min({c*a, c*b, d*a, d*b}), mx = max({c*a, c*b, d*a, d*b});
	else if(chr == 'd') {
		mn = c;
		mx = d*b;
	}
	num.push({mn, mx});
}

void tobe(){

	for(int i = 0;i < s.size(); ++i) {
		if(isdigit(s[i])){    //读取数字
			int tp = 0, j = i;
			for(;j < s.size();++j){
				if(isdigit(s[j])) tp = tp * 10 + s[j] - '0';
				else break;
			}

			i = j - 1;
			num.push({tp, tp});
//
		}else{
			if(s[i] == '(') ch.push(s[i]);
			else if(s[i] == 'd') {    //右结合, 遇到同级符号不需要计算
				ch.push(s[i]);
			}else if(s[i] == '*') {   //左结合, 遇到同级或上级符号, 需要先算栈顶符号
				while(!ch.empty() && (ch.top() == 'd' || ch.top() == '*')) cal();
				ch.push(s[i]);
			}else if(s[i] == '+' || s[i] == '-') {
				while(!ch.empty() && (ch.top() == 'd' || ch.top() == '*' || ch.top() == '+' || ch.top() == '-')) cal();
				ch.push(s[i]);
			}else if(s[i] == ')') {  //(括号处理
				while(!ch.empty() && ch.top() != '(') cal();
				ch.pop();
			}
		}
	}
}

signed main() {

//	freopen("an", "r", stdin);
	caillo;

	while(cin >> s) {
		while(!num.empty()) num.pop();
		while(!ch.empty()) ch.pop();

		tobe();
		while(!ch.empty()) cal();  //完全计算
		int ansl = num.top().first;
		int ansr = num.top().second;
		cout << ansl << ' ' << ansr << endl;
	}

	PLEASE_AC;
}
```