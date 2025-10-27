---
title: CS61A lecture 0 function note
date: 2025-09-30 10:16:22
categories: [note, cs61a]
tags: cs61a
---



### 一、表达式与调用

* **表达式（expression）**：能被求值并产生一个值。
* **调用表达式（call expression）**：

  * 形式：`<operator>(<operand1>, <operand2>, …)`
  * 先求值操作数，再调用操作符（即函数对象）。
  * 示例：

    ```py
    add(2, mul(3, 4))  # 先计算 mul(3, 4) → 12，再计算 add(2, 12) → 14
    ```

---

### 二、函数（Functions）

#### 1. 名字与绑定（Name Binding）

绑定发生于：

1. **导入（import）**：如 `from math import sqrt`
2. **赋值（assignment）**：如 `f = sqrt`
3. **定义（def）**：如 `def f(x): ...`

---

#### 2. 定义函数（Defining Functions）

语法：

```py
def <name>(<formal parameters>):
    """函数说明文档。
    描述用途、参数、返回值等。
    """
    return <return expression>
```

设计建议：

* 写文档字符串（docstring），可用 `help(f)` 查看。
* 默认参数放在非默认参数之后。

---

### 三、None 与语句（Statements）

* `None` 表示“无返回值”。
* **语句类型**：赋值、`def`、`return`
* **区别**：语句被执行，表达式被求值。
* 表达式可作为语句执行，此时结果被丢弃。
* `return` 语句的表达式在函数调用时求值。

---

### 四、纯函数与非纯函数

* **纯函数（Pure Function）**：仅返回值，无副作用。
* **非纯函数（Non-pure Function）**：除了返回值，还会影响外部状态（如 `print`）。

---

### 五、局部函数（Local Function）

函数仅能访问其局部 frame 中的变量。

---

### 六、断言（Assertions）

* 用于调试和验证假设。
* 示例：

  ```py
  assert fib(7) == 13, 'the seventh fib is 13'
  ```
* 若条件为假，抛出错误并输出消息。

---

### 七、文档测试（Doctest）

* 可在 docstring 中写示例并自动测试。
* 运行方式：

  ```bash
  python3 -m doctest <python_file> -v
  ```
