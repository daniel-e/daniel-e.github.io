---
layout: post
title: Python Cheat Sheet of not so well-known stuff
tags: [python]
---
<div style="font-size:small; color: gray; font-style: italic">
  By <a href="https://twitter.com/dnl0x00">@dnl0x00</a>
</div>

### Non-keyworded, variable length argument list.
Here's an example which shows how to pass variable length arguments and get the values.

{% highlight python linenos %}
def g(a, *args):
    b, c = args
    assert a == 1

def f(a, *args):
    g(a, *args)

f(1, 2, 3)
{% endhighlight %}

### for ... else

Did you know that the `for` loop in Python has an `else`? The `else` part is executed if the `for` was not quit via a break.

{% highlight python linenos %}
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
{% endhighlight %}

### toString for Python objects

Want a `toString` like in Java for objects? Implement `__repr__`.

{% highlight python linenos %}
class X:
    def __repr__(self):
       return "This is X"

x = X()
print(x)
{% endhighlight %}

### Expanding tuples into arguments

{% highlight python linenos %}
def f(a, b, c):
    pass

params = (1, 2, 3)
f(*params)
{% endhighlight %}

### () operator in Python

{% highlight python linenos %}
class X:
    def __call__(self, x):
       return x + 1

f = X()
print(f(5))  # print the value 6
{% endhighlight %}

### fileinput

{% highlight python linenos %}
import filename
for line in fileinput.input():
    process(line)
{% endhighlight %}

"This iterates over the lines of all files listed in sys.argv[1:], defaulting to sys.stdin if the list is empty. If a filename is '-', it is also replaced by sys.stdin."

source: https://docs.python.org/2/library/fileinput.html
