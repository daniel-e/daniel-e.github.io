---
layout: post
title: MNIST handwritten digits for Python
tags: [machine learning, python]
---
<div style="font-size:small; color: gray; font-style: italic">
  By <a href="https://twitter.com/dnl0x00">@dnl0x00</a>
</div>

I've recently created a small Python library [mnistdb](https://github.com/daniel-e/mnistdb) which can be used to easily load the [MNIST database of handwritten digits](http://yann.lecun.com/exdb/mnist/) in Python into numpy arrays without any manual effort. It works for Python 2 and Python3.

Just install the library via pip:

```
pip install mnistdb
```

Here's an example which shows how to use it:

{% highlight python linenos %}
import mnistdb.io as mio

data = mio.load()

print(data.trainX.shape)
print(data.trainY.shape)
print(data.testX.shape)
print(data.testY.shape)
{% endhighlight %}

When you're running the code for the first time mnistdb will download the MNIST database of handwritten digits from the Internet. The database will be stored in ~/.mnistdb so that does not need to download the database for subsequent calls.

As you can see the database can be accessed easily. Auto-completion should be automatically be supported by your IDE.
