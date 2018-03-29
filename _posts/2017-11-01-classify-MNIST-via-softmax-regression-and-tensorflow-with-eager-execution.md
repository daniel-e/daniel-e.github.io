---
layout: post
title: Classify MNIST via Softmax Regression and TensorFlow with eager execution
tags: [machine learning, python]
---
<div style="font-size:small; color: gray; font-style: italic">
  By <a href="https://twitter.com/dnl0x00">@dnl0x00</a>
</div>

Recently, Google announced the [eager execution for TensorFlow](https://research.googleblog.com/2017/10/eager-execution-imperative-define-by.html). Eager execution has some advantages when doing quick prototyping. The code is easier to debug because operations are executed immediately and you can build models via Python control flow (including `if` statements and `for` and `while` loops). You don't have to deal with those graphs and sessions anymore which makes the code more compact, readable and intuitive.

Currently, the TensorFlow version with eager execution is only available via a nightly. You can install the version via pip.

```
pip install tf-nightly
```

Here's a minimal example on how you do softmax regression with TensorFlow with eager execution.

First, import all required modules.

{% highlight python linenos %}
import tensorflow as tf
import tensorflow.contrib.eager as tfe
import numpy as np
import mnistdb.io as mio
{% endhighlight %}

Enable eager mode.

{% highlight python linenos %}
tfe.enable_eager_execution()
{% endhighlight %}

Load the MNIST database via the `mnistdb` module.

{% highlight python linenos %}
data = mio.load(scaled=True, one_hot=True)
X_tr, y_tr, X_te, y_te = (data.trainX, data.trainY, data.testX, data.testY)
{% endhighlight %}

Create the weights and biases which need to be optimized. At the moment we have to assign a name to each created variable manually. This might change for the next builds so that probably we could omit the `name` parameter in the future.

{% highlight python linenos %}
W = tfe.Variable(tf.random_normal(shape=(28*28, 10)), name="W")
b = tfe.Variable(tf.zeros((1, 10)), name="b")
{% endhighlight %}

Define a function which computes the prediction accuracy for the given examples X and their known one-hot values y.

{% highlight python linenos %}
def acc(X, y):
    z = tf.nn.softmax(tf.matmul(X, W) + b)
    # workaround as tf.metrics.accuracy is not available in eager mode (yet)
    lbls = tf.argmax(y, axis=1).numpy()
    pred = tf.argmax(z, axis=1).numpy()
    return sum([a == b for a, b in zip(lbls, pred)]) / len(lbls)
{% endhighlight %}

Define the loss function which computes the loss for the given examples X and their known one-hot values y.

{% highlight python linenos %}
def loss(X, y):
    z = tf.nn.softmax(tf.matmul(X, W) + b)
    return tf.reduce_mean(-tf.reduce_sum(y * tf.log(z), reduction_indices=[1]))
{% endhighlight %}


Create an optimizer.

{% highlight python linenos %}
optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.1)
grad = tfe.implicit_gradients(loss)
{% endhighlight %}

Train the model.

{% highlight python linenos %}
for i in range(100):
    print("loss on training data {}, on test data {}".format(loss(X_tr, y_tr).numpy(), loss(X_te, y_te).numpy()))
    print("accuracy on test data", acc(X_te, y_te))
    # The parameters given to grad are passed to our loss function defined above.
    optimizer.apply_gradients(grad(X_tr, y_tr))
{% endhighlight %}


Here's the complete code:

{% highlight python linenos %}
import tensorflow as tf
import tensorflow.contrib.eager as tfe
import numpy as np
import mnistdb.io as mio

tfe.enable_eager_execution()

data = mio.load(scaled=True, one_hot=True)
X_tr, y_tr, X_te, y_te = (data.trainX, data.trainY, data.testX, data.testY)

W = tfe.Variable(tf.random_normal(shape=(28*28, 10)), name="W")
b = tfe.Variable(tf.zeros((1, 10)), name="b")

def acc(X, y):
    z = tf.nn.softmax(tf.matmul(X, W) + b)
    # workaround as tf.metrics.accuracy is not available in eager mode (yet)
    lbls = tf.argmax(y, axis=1).numpy()
    pred = tf.argmax(z, axis=1).numpy()
    return sum([a == b for a, b in zip(lbls, pred)]) / len(lbls)

def loss(X, y):
    z = tf.nn.softmax(tf.matmul(X, W) + b)
    return tf.reduce_mean(-tf.reduce_sum(y * tf.log(z), reduction_indices=[1]))

optimizer = tf.train.GradientDescentOptimizer(learning_rate=0.1)
grad = tfe.implicit_gradients(loss)

for i in range(100):
    print("loss on training data {}, on test data {}".format(loss(X_tr, y_tr).numpy(), loss(X_te, y_te).numpy()))
    print("accuracy on test data", acc(X_te, y_te))
    optimizer.apply_gradients(grad(X_tr, y_tr))
{% endhighlight %}

The code is also available on [GitHub](https://github.com/daniel-e/ml/blob/master/tf_eager_softmax_regression.py).
