---
layout: post
title: MNIST database of handwritten digits for Octave
---

In this post you will see how to convert the MNIST database of handwritten digits via [rustml](/rustml/rustml/) into a format that can be read with [Octave](https://www.gnu.org/software/octave/).

The MNIST database of handwritten digits (see [here](http://yann.lecun.com/exdb/mnist/)) is a very popular dataset used by the machine learning research community for testing the performance of classification algorithms. It contains 60,000 labeled training examples of handwritten digits between 0 and 9 (both including) and 10,000 labeled examples for testing. Each digit is represented as a grayscale image each with a width and height of 28 pixels. The value of a pixel is in the interval [0, 255].

Because the database from the link above is in a format that cannot be directly processed with Octave we will use [rustml](/rustml/rustml) to convert it into a format that can be read with Octave.

## Download the dataset 
First, we need to download the original dataset. As described [here](https://github.com/daniel-e/rustml#rustml-datasets-package) we execute the following commands on the command line:

<pre><code class="bash"># download the installer script
wget -q https://raw.githubusercontent.com/daniel-e/rustml/master/dl_datasets.sh
# execute the script
bash ./dl_datasets.sh
</code></pre>

The installer script will download the datasets into the directory `~/.rustml/`.

## Converting the dataset

Before you can perform the following steps you have to install the Rust programming language. If not already installed, download the latest version from [rust-lang.org](https://www.rust-lang.org) and execute the following commands on the command line. Skip the instructions in the box below if Rust is already installed.

<pre><code class="bash"># unpack the archive (the name of the archive may depend on your system configuration)
tar xzf ~/Downloads/rust-VERSION-x86_64-unknown-linux-gnu.tar.gz

# change into the directory
cd rust-VERSION-x86_64-unknown-linux-gnu/

# execute the install script with the destination directory
./install.sh --prefix=/opt/rust-VERSION

# add the rust directory to the search path
export PATH=/opt/rust-VERSION/bin:$PATH
</code></pre>

If Rust was installed successfully we create a new project with cargo:

<pre>
cargo new --bin convert
</pre>

Append the following two lines (which specify the required dependencies and where to search for them) to `Cargo.toml` which is located in the directory created by the command above.

<pre>
[dependencies]
rustml = { git = "https://github.com/daniel-e/rustml.git" }
</pre>

Put the following content into the file `main.rs` which is located in the directory `src`:

<pre><code class="rust">extern crate rustml;

use std::fs::File;
use std::io::Write;

use rustml::datasets::MnistDigits;
use rustml::io::OctaveFormat;

fn main() {

    let (train_x, train_y) = MnistDigits::default_training_set().unwrap();
    let (test_x, test_y) = MnistDigits::default_test_set().unwrap();

    let s = train_x.to_octave_string("trainX") +
        &train_y.to_octave_string("trainY") +
        &test_x.to_octave_string("testX") +
        &test_y.to_octave_string("testY");

    File::create("mnist.txt")
        .unwrap()
        .write_all(s.as_bytes())
        .unwrap();
}
</code></pre>

You can also download the sources directly:
<pre><code class="bash">git clone git@github.com:daniel-e/blogdata.git
cd blogdata/mnist2octave/
</code></pre>

Now, the program can be build and executed as follows:

<pre>
cargo run
</pre>

## Result

The result is written into the file `mnist.txt` in the current working directory. Because the size of the file is roughly 120MB and Octave can also handle gzip compressed files we compress the text file with the command `gzip mnist.txt` to reduce its size. The result is a file `mnist.txt.gz` with a size of roughly 15MB.

Finally, this file can be loaded via the `load` function in Octave.

<pre><code class="matlab">octave:1> load("mnist.txt.gz");
octave:2> who
Variables in the current scope:

testX   testY   trainX  trainY

octave:3> size(trainX)
ans =

   60000     784
</code></pre>

Each digit can be visualized in Octave very easily. For example, to display the digit at row 5 of the training examples stored in the matrix `trainX` simply do the following:

<pre><code class="matlab">octave:1> imagesc(reshape(trainX(4, :), 28, 28)');
</code></pre>

This will show the image below:

![plot of a digit of the mnist database](/assets/nine.png)

## Sources

All sources and the MNIST database for Octave is available on [GitHub](https://github.com/daniel-e/blogdata/tree/master/mnist2octave).

