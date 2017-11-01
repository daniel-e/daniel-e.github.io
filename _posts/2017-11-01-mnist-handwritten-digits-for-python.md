---
layout: post
title: MNIST handwritten digits for Python
tags: [machine learning, python]
---

I've recently created a small Python library [mnistdb](https://github.com/daniel-e/mnistdb) which can be used to easily load the [MNIST database of handwritten digits](http://yann.lecun.com/exdb/mnist/) in Python into numpy arrays without any manual effort.

Just install the library via pip:

```
pip install mnistdb
```

Here's an example which shows how to use it:

```
import mnistdb.io as mio

data = mio.load()

print(data.trainX.shape)
print(data.trainY.shape)
print(data.testX.shape)
print(data.testY.shape)
```

When you're running the code for the first time mnistdb will download the MNIST database of handwritten digits from the Internet. The database will be stored in ~/.mnistdb so that does not need to download the database for subsequent calls.

As you can see the database can be accessed easily. Auto-completion should be automatically be supported by your IDE.
