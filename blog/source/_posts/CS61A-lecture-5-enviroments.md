---
title: CS61A lecture 5 enviroments
date: 2025-10-05 18:03:53
categories: [note, cs61a]
tags: cs61a
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
lambda              x         :              f(g(x))
"A function that    takes x   and returns    f(g(x))"
```

## Function Currying
给定一个函数 f(x, y)，我们可以定义另一个函数 g 使得 g(x)(y) 等价于 f(x, y)。在这里，g 是一个高阶函数，它接受单个参数 x 并返回另一个接受单个参数 y 的函数。这种转换称为柯里化（Curring）。

## return statemenst
A return statement completes the evaluation of a call expression and provides its value f(x) for user-defined function f: switch to a new environment; execute f's body  
return statement within f: switch back to the previous environment; f(x) now has a value
## closure

> **闭包（closure）** 是一个函数（通常是内部函数），它“记住”了**定义时**所在的外部作用域中的变量，即使外部函数已经执行完毕。

> 闭包 = **函数本身** + **它能访问的外部变量环境（enclosing environment）**

---

### 🍰 举个例子（直观理解）

```python
def outer():
    x = 10
    def inner():
        print(x)
    return inner

f = outer()
f()   # 输出 10
```

### 执行过程：

1. 调用 `outer()`：

   * 创建局部变量 `x = 10`
   * 定义内部函数 `inner()`
   * 返回 `inner` 函数对象（不是执行它！）

2. 即使 `outer()` 执行结束、它的局部变量 `x` 按理说应该被销毁，
   但因为 `inner()` 引用了 `x`，Python 会把 `x` **保存在闭包里**。

3. 当执行 `f()` 时，`inner` 仍然能访问 `x=10`。

---

### ⚙️ 闭包的三个必要条件

要形成闭包，必须满足以下条件：

1. **函数嵌套**（函数里定义函数）

   ```python
   def outer(): 
       def inner(): ...
   ```

2. **内部函数引用了外部函数的变量**

   ```python
   x = 10
   def inner(): print(x)
   ```

3. **外部函数返回内部函数**

   ```python
   return inner
   ```

满足这三点，就形成闭包。

---

### 🧩 闭包的价值

闭包能让函数“记住”状态，非常适合用于：

###  1. 数据封装

不用类就能保存数据。

```python
def make_counter():
    count = 0
    def add_one():
        nonlocal count
        count += 1
        return count
    return add_one

counter = make_counter()
print(counter())  # 1
print(counter())  # 2
print(counter())  # 3
```

这里 `count` 永远“活”在 `add_one` 的闭包中。

---

###  2. 延迟计算

```python
def multiplier(n):
    return lambda x: x * n

times3 = multiplier(3)
print(times3(5))  # 15
```

`times3` 记住了 `n=3`，即使外层函数 `multiplier` 早就结束。

---

###  3. 函数工厂（function factory）

闭包经常用于生成一类带记忆的函数，比如：

```python
def power(p):
    return lambda x: x ** p

square = power(2)
cube = power(3)

print(square(4))  # 16
print(cube(4))    # 64
```

---
