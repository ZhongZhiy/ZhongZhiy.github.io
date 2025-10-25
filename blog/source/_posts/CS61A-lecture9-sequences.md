---
title: CS61A-lecture9-sequences
date: 2025-10-18 17:08:56
categories: [note, cs61a]
tags: [note]
---

# sequences
## list
python list

```py
digits = [1, 8, 2, 8]
[2, 7] + digits * 2
pairs = [[10, 20], [30, 40]]
```

## containers
it just search element by element
```py
>>> 1 in digits
True
```

## for statements

```py
def count(s, value):
  """count the numberof times that value in sequence s.
  """"

  total, index = 0, 0
  while index < len(s):
    element = s[index]
    if element == value:
      total = total + 1
    index = index + 1
  return total
```

## range
`range(1, n)` from `1` to `n - 1`

## list comprehesion
```py
[x * x for x in sets if x % 25 == 0]
```
## lists, Slices & Recursion
### expamle
```py
def sum_list(s):
  """sum the elements of list s.
  """
  if len(s) == 0:
    return 0
  else:
    return s[0] + sum_list(s[1:])
```
# containers
## box-and-pointer Notation
### the closure property of data types
1. 操作的结果仍然属于相同的类型
2. 闭包的强大之处在于：它允许我们构建层级结构（hierarchical structures）。
3. 层级结构的本质是“部分由部分组成”。
4. 在 Python 里，列表是一种具有闭包性质的数据结构

### box-and-pointer Notation in environment diagrams
{% asset_img image.png %}

## slicing
```py
odds = [3, 5, 7, 9, 11]
[odd[i] for i in range(1, 3)]
```

## precessing container values
there are some build-in function
### sequence aggregation

- `sum(literable [, other])

- 'max(literable [, key])
