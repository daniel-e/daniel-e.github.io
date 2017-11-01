---
layout: post
title: Python Cheat Sheet of not so well-known stuff
tags: [python]
---

### Non-keyworded, variable length argument list.
Here's an example which shows how to pass variable length arguments and get the values.

```python
def g(a, *args):
    b, c = args
    assert a == 1
    assert b == 2
    assert c == 3

def f(a, *args):
    g(a, *args)

f(1, 2, 3)
```

### for ... else

Did you know that the `for` loop in Python has an `else`? The `else` part is executed if the `for` was not quit via a break.

```python
def chk(n):
    for i in range(60):
        if i == n:
            break
    else:
        # this code is executed only if the for loop was
        # not quit via break
        return 1
    return 0

assert chk(30) == 0
assert chk(90) == 1
```

### toString for Python objects

Want a `toString` like in Java for objects? Implement `__repr__`.

```python
class X:
    def __repr__(self):
       return "This is X"

x = X()
print(x)
```
