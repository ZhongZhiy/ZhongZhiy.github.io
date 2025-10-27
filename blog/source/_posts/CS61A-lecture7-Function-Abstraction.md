---
title: CS61A-lecture7-Function-Abstraction
date: 2025-10-11 22:11:47
categories: [note, CS61a]
tags: cs61a
---
# Function Abstractions
eg:
```py
def square(x):
  return mul(x, x)

def sum_squares(x, y):
  return square(x) + square(y)
```
What dos sum_squares need to know about square?
- square need to know square exactly take one argument
- no need to know the intrinsic name square, it can be bonded to any name
- need to know the behavior of the square
- do not need to know square compute the square by calling mul

## choosing name
name typically don't matter fo correctness
but they matter a lot for composition
- name should convey the meaning or purpose of the values to which they are bound.
- the type of value bound to the name is best documented in a function's docstring.
- function names typically convey their effect , their behavior , or the value returned.

## Error & tracebacks
### error
- syntax errors, caused by expression not formal
- run time error, it can be see a traceback
- logical error, write test to check code
