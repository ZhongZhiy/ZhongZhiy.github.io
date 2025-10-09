---
title: CS61A lecture 5 enviroments
date: 2025-10-05 18:03:53
categories:
tags:
---
# environment
调用一个用户函数:
1. create a new frame
2. bind 两个实参到形参上
3. 执行函数体
## nested definitions嵌套定义
1. 每个用户函数都有parent frame(often global)
2. 函数的parent 就是定义它的frame
3. 每个local frame 有一个parent frame(often global)
4. the parent of a frame is the paret of the function called

## lambda 表达式
```py
square = lambda x : x * x
```

## Function Currying
