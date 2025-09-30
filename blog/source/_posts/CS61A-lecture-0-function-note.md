---
title: CS61A lecture 0 function note
date: 2025-09-30 10:16:22
categories:
tags:
---

# 表达式
## 表达式的调用

{% asset_img call_exp.png %}

# function

## name 
buildin funtion
```python
from math import pi
pi
```

bound :
1. import buildin name
2. assignment statement
3. def

## defining functions
```py
def <name>(<formal parameters>):
  return <return expression>
```

## environment diagrams

## 
`None` reprsents nothing, that a funcion does not return a value will return `None`

### Pure / Non-pure funcion
Pure funcion: just return values
{% asset_img function_abs.png %}

Non-pure function: 除了返回值外，调用一个非纯函数还会产生其他改变解释器和计算机的状态的副作用（side effect）。一个常见的副作用就是使用 print 函数产生（非返回值的）额外输出。
{% asset_img funtion_print.png %}