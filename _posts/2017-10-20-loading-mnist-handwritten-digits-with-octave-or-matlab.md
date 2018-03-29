---
layout: post
title: Loading MNIST Handwritten Digits With Octave or MATLAB
tags: [machine learning]
---
<div style="font-size:small; color: gray; font-style: italic">
  By <a href="https://twitter.com/dnl0x00">@dnl0x00</a>
</div>

The MNIST handwritten digit database is a very popular data set for testing machine learning algorithms. It contains 60,000 labeled training examples and 10,000 examples for testing. The data set can be downloaded from [here](http://yann.lecun.com/exdb/mnist/). On GitHub I have published a [repository](https://github.com/daniel-e/mnist_octave) which contains a file `mnist.mat` created from this raw data set which can easily be loaded with Octave or MATLAB so that you can easily use the data set in Octave or MATLAB.

Either you can use this file directly or you can create it with the `mnist.py` Python script contained in this repository.

## Loading the data set directly in Octave/MATLAB

This step does not cloning the repository. You just have to download one file.

Download the file:

    wget https://github.com/daniel-e/mnist_octave/raw/master/mnist.mat

Start Octave and type:

```matlab
d = load('mnist.mat');
```

Now, `d` is a struct which contains the training and testing examples with the corresponding labels:
<ul>
<li> d.trainX is a (60000,784) matrix which contains the pixel data for training</li>
<li> d.trainY is a (1,60000) matrix which contains the labels for the training data</li>
<li> d.testX is a (10000,784) matrix which contains the pixel data for testing</li>
<li> d.testY is a (1,10000) matrix which contains the labels for the test set</li>
</ul>

You could now display a single example. For example, to display the third example of the training set type the following:

```matlab
X = d.trainX;
i = reshape(X(3,:), 28, 28)';
image(i);
```


## Create an Octave/MATLAB file from the raw data set

For this step it is recommended to clone the repository via:

    git clone https://github.com/daniel-e/mnist_octave.git

First, you require some Python packages. This step is optional if you already have all packages installed. You can either install the required packages directly into your system with `pip` or you can create a virtual environment into which the packages are installed.

**Install packages directly**

    pip3 install scipy numpy matplotlib

**Install the packages into a virtual environment**

    virtualenv -p python3 venv
    # activate the virtual environment
    source venv/bin/activate
    pip3 install --upgrade pip
    pip3 install scipy numpy matplotlib


**Download the raw data set**

If you cloned the GitHub repository this step is optional as the repository already contains the files. If you haven't cloned it you have to download the data set from [http://yann.lecun.com/exdb/mnist/](http://yann.lecun.com/exdb/mnist/).

    wget http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz
    wget http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz
    wget http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz
    wget http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz

**Execute mnist.py to create a matrix for Octave and MATLAB.**

    ./mnist.py

A new file `mnist.mat` is created which contains the data. This matrix can now be loaded with Octave or MATLAB as described above.
