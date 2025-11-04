---
title: CS61A-lecture13-data-abstraction
date: 2025-10-31 20:54:43
categories: [note, cs61a]
tags: [cs61a]
---

# Data Abstraction
An abstract data type lets us manipulate compound objects as nuits
Islolate two parts of any program that uses data:
- how data are represented
- how data are manipulated
分成两部分后, 改变一部分不会影响另外一部分, 从而实现数据的抽象, 有利于程序模块化
Data abstraction : A methodology by which functions enforce an abstraction barrier between representation and uses
**Example: Rational Numbers**:
```py
def mul_rational(x, y):
    return rational(numer(x) * numer(y), denom(x) * denom(y))
def add_rational(x, y):
    return rational(numer(x) * denom(y) + numer(y) * denom(x), denom(x) * denom(y))
def equal_rational(x, y):
    return numer(x) * denom(y) == numer(y) * denom(x)
```
我们可以根据这些函数之后再实现数据结构

# Pairs
consist of two elements
## Representing pairs using lists
```py
pair = [1, 2]
x, y = pair
pair[0]
from operator import getitem
getitem(pair, 0)
```
## Representing Rational Numbers
```py
def rational(n, d):
    """construct a rational number that represents N/D."""
    return [n, d]

def numer(x):
    """return the numerator of rational number x."""
    return x[0]

def denom(x):
    """return the denominator of rational number x."""
    return x[1]
```

## reducing to lowest terms
```py
from fractions import gcd
def rational(n, d):
    g = gcd(n, d)
    return [n//g, d//g]

```

# Abstraction barriers
Abstraction barriers separate different parts of a program so that each part only needs to so much about rest of program. these separations are important because they allow you to make changes to one part of your program and have other parts take advantage fo those changes without breaking in any way or creating inconsistencies.
{% asset_img abstraction_barrier.png %}
每条线都是一个abstraction barrier, 当写大型项目的时候, 使用适当的抽象级别
在不跨越这些界限的情况下越容易向上改变你的程序

## Violating Abstraction barriers
```py
add_ration([1, 2], [1, 4])

def divide_rational(x, y):
    return [x[0] * y[1], x[1] * y[0]]
```
{% asset_img violate.png %}
在上面的每一层中，最后一列中的函数会强制实施抽象屏障（abstraction barrier）。这些功能会由更高层次调用，并使用较低层次的抽象实现。

当程序中有一部分本可以使用更高级别函数但却使用了低级函数时，就会违反抽象屏障。
# Data Representations
The purpose of maintaining barriers is so that you can change your data representation without having to rewrite your entire program.

## Data
data abstration recognized by its behavior
我们可以使用选择器和构造器的集合以及一些行为条件来表达抽象数据。
