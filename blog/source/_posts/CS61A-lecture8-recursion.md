---
title: CS61A-lecture8-recursion
date: 2025-10-15 10:26:33
categories: [note, cs61a]
tags: note
---

# recursion
```py
def print_sums(x):
  print(x)
  def nest_sum(y):
    return print_sums(x + y)
  return next_sum

print_sums(1)(3)(5)
```

---

## reursive function
- **definition**: a function is called *recursive* if the body of that function calls itself, either directly or indirectly
- **Implication**: executing the body of a recursive function may require applying that function again.
### digit sum
```py
def split(n):
  """split positve n into all but its last digit and its last digit"""
  return n // 10, n % 10

def sum_digits(n):
  if n < 10:
    return n
  else:
    all_but_last, last = split(n)
    return sum_digits(all_but_last) + last
```

---

## the recursive leap of faith
```py
def fact(n):
  if n == 0:
    return 1
  else:
    return n * fact(n - 1)
```
Is factimplemented correctly?
1. verify th base case
2. treat *fact* as a functional abstraction
3. assume that *fact(n-1)* is correct
4. verify that *fact(n)* is correct, assuming that *fact(n-1)* correct

---

## mutual recursion

### the luhn algorithm
used to verify credit card numbers
the sum equl to double even position digit add odd potion digit, the sum is times of 10
```py
def luhn_sum(n):
  if n < 10:
    return n
  else:
    all_but_last, last = split(n)
    return luhn_sum_double(all_but_last) + last

def luhn_sum_double(n):
  all_but_last, last = split(n)
  luhn_digit = sum_digits(2 * last)
  if n < 10:
    return luhn_dight
  else:
    return luhn)sum(all_but_last) + luhn_digit
```

---
## conberting recursion to lteration

---

# order of recursion calls
understanding recursion function behavior

illustraion:
```py
def cascade(n):
  if n < 10:
    pirnt(n)
  else:
    print(n)
    cascade(n//10)
    print(n)

cascade(123) 
```
- Each cascade frame is from a different call to cascade.
- Until the return value appears, that call has not completed.
- Any statement can appear before or after the recursive call.(print)

### example: inverse cascade
prints an inverse cascade
```
1
12
123
1234
123
12
1
```

```py
def inverse_cascade(n):
  grow(n)  #1 // 12 // 123
  print(n)  #/1234
  shrink(n) #123 //12// 1

def f_then_g(f, g, n):
  if n:
    f(n)
    g(n)

grow = lambda n : f_then_g(grow, print(n), n // 10)
shrink = lambda n : f_then_g(print(n), shrink, n // 10)
```

---

# tree recursion
tree-shaped processes arise whenever executing the body of a recursive function makes more than one cal to than function

```py
def fib(n):
  if n == 0:
    return 0
  elif n == 1:
    return 1
  else:
    return fib(n - 2) + fib(n - 1) 
```
### expamle: counting partitions
```py
count_partions(6, 4)
2 + 4 = 6
1 + 1 + 4 = 6
3 + 3 = 6
1 + 2 + 3 = 6
1 + 1 + 1 + 3 = 6
2 + 2 + 2 = 6
1 + 1 + 2 + 2 = 6
1 + 1 + 1 + 1 + 2 = 6
1 + 1 + 1 + 1 + 1 + 1 = 6
=> 9
```
strategy:
- recursive decomposition: finding simpler instances of the problem.
- explore two possiblities:
  - use at least one 4
  - don't use any 4
- solve two simpler porblems:
- count_partitions(2, 4)
- count_partitions(6, 3)

```py
def count_partitions(n, m):
  if n == 0:
    return 1
  elif n < 0:
    return 0
  elif: m == 0:
    return 0
  else:
    with_m = count_partitions(n - m, m)
    without_m = count_parition(n, m - 1)
    return with_m + without_m
```

---


