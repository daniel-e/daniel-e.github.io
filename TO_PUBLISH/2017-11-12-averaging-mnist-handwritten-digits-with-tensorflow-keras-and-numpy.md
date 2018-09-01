---
layout: post
title: Averaging MNIST handwritten digits with TensorFlow, Keras and Numpy
tags: [machine learning, python]
---

to setup an environment see other post
TF 1.4

import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

def main():
    (x_train, y_train), (_x_test, _y_test) = tf.keras.datasets.mnist.load_data("mnist.db")

    assert x_train.shape == (60000, 28, 28)
    assert y_train.shape == (60000,)

    # number of examples in the training dataset
    n = x_train.shape[0]
    # get all examples for which the label is 3
    a = np.array([x_train[i, :, :].reshape((28, 28)) for i in range(n) if y_train[i] == 3])
    # average all examples with label 3
    m = np.mean(a, axis=0)

    plt.imshow(m, cmap="gray")
    plt.show()

if __name__ == '__main__':
    main()
