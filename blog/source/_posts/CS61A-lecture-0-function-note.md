---
title: CS61A lecture 0 function note
date: 2025-09-30 10:16:22
categories:
tags:
---

# 表达式与调用

- 表达式（expression）会被求值，产生一个值。
- 调用表达式形如：<operator>(<operand1>, <operand2>, ...)。先求值操作数（operands），再将它们传给作为操作符（operator）的可调用对象。

示例：

```py
def add(x, y):
  return x + y

def mul(x, y):
  return x * y

add(2, mul(3, 4))  # 先算 mul(3, 4) -> 12，再算 add(2, 12) -> 14
```

{% asset_img call_exp.png %}

# Functions（函数）

## 名字与绑定（name binding）

名字与对象的绑定常见于：
1. 导入（import）：导入内建/库中的名字，例如 `from math import sqrt`。
2. 赋值（assignment）：`f = sqrt`。
3. 定义（def）：`def f(x): ...`。

## 定义函数（defining functions）

```py
def <name>(<formal parameters>):
  """一行描述函数的任务。

  可以在接下来的若干行说明参数、返回值与边界情况。
  """
  return <return expression>
```

### 函数设计建议
1. 写文档字符串（docstring）：用三引号描述用途与参数。可用 `help(function)` 查看。
2. 合理使用默认参数：带默认值的形参放在无默认值形参之后。


## None
`None` reprsents nothing, that a funcion does not return a value will return `None`
## 语句
包括: 赋值(assignment), def, return语句
语句不会被求解, 而会被执行, 被执行的语句会求解其包含的子表达式, return 表达式不会立刻求值, 二十储存起来供以后调用该函数时使用
表达式也可以作为语句执行，在这种情况下，它们会被求值，但它们的值会被丢弃。
执行纯函数没有效果, 执行非纯函数产生效果
### Pure / Non-pure funcion
Pure funcion: just return values
{% asset_img function_abs.png %}

Non-pure function: 除了返回值外，调用一个非纯函数还会产生其他改变解释器和计算机的状态的副作用（side effect）。一个常见的副作用就是使用 print 函数产生（非返回值的）额外输出。
{% asset_img funtion_print.png %}

## 局部函数
函数只能操纵器局部frame

## 断言assertions
`assert fib(7) == 13, 'the seventh fib is 13' #if it's false, it will print the secentens

## 文档测试doctests
```py
python3 -m doctest <python_file> -v
```
