---
title: CS61A lecture 4 Higher_Order Function
date: 2025-10-04 10:44:06
categories: note
tags: note
---
# Higher_Order Function
把函数作为参数或者返回值的函数
高阶函数是一种比用户定义函数更为强大的抽象: 用一些函数来表达计算的通用方法, 而且和他们调用的特定函数无关


### Cntrol
use function to control
```py
if __:
  ____
eles ___:
_____
```
if we use funciotn `if(____, ___, ____)` 
evaluation rule for call expressions is quite different than for condiction statements
#### the difference 

函数不会跳过调用, 表达式可以跳过, 或者不执行某些分支

## Control Expressions
一些可以跳过的表达式
1. 逻辑运算的短路逻辑, `and` and `or`

## Higher-Order Functions
`assert 3 > 2, "Math is broken" `, 如果断言为false, 就会返回错误
计算, 计算过程
## Generalization
{% asset_img term.png %}

## 函数返回函数
```py
def make_adder(n):
  def adder(k):
    return k + n
  return adder
```
call `make_adder(1)(2)`
{% asset_img image.png %}

## Purpose
Funciton are first_class object 可以作为值

Higger_order funtion: 把函数作为一个参数或者返回值

# reading
## 1.6 高阶函数
- 函数可以作为参数传递, 通过name来抽象
- 函数作为通用方法来表达计算
- 局部函数可以访问其父环境
- 在像 Python 这样的带有词法作用域的语言里，当一个函数在另一个函数内部被定义并返回时，它会“带着”定义时的外部变量环境一起返回。即使外层函数已经结束执行，内部函数仍然能访问那些外层变量。

### 柯里化curring
我们可以使用高阶函数将一个接受多个参数的函数转换为一个函数链，每个函数接受一个参数。

### lambda表达式
一个 lambda 表达式的计算结果是一个函数，它仅有一个返回表达式作为主体。不允许使用赋值和控制语句。
```
lambda              x         :              f(g(x))
"A function that    takes x   and returns    f(g(x))"
```

### decorator
```py
@函数名
def 原函数名(...):
    ...
```

等价于:
```py
原函数名 = 函数名(原函数名)
```

