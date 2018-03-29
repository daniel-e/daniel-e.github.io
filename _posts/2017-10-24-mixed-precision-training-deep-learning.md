---
layout: post
title: Mixed Precision Training
tags: [paper, machine learning]
---
<div style="font-size:small; color: gray; font-style: italic">
  By <a href="https://twitter.com/dnl0x00">@dnl0x00</a>
</div>

In October this year researchers from Baidu and NVIDIA have published a [paper](https://arxiv.org/pdf/1710.03740.pdf) in which they describe a technique to train deep neural networks using half precision floating point numbers (FP16) to decrease the memory consumption of the models and to benefit from speed up effects due to that.

A general rule of thumb in deep learning is that the accuracy achieved by a model becomes better the more complex the model is and the more data is used. As a result, models today are quite complex and consume a large amount of memory. Currently, mainly FP32 is used during the training of a model and techniques to reduce training to FP16 were not very successful. Often the models were less accurate or it is unclear how they work on complex models. In this paper a technique is presented which also works for very complex models. All tensors and operations are FP16 and hyper-parameters do not have to be adjusted. By reducing to FP16, memory bandwidth can be used more efficiently (which often is a bottleneck in training), less memory is required and operations can work on more values in parallel.

The main problems when moving from FP32 to FP16 are: 1) weight updates could become too small to be represented in FP16 (e.g. if values become smaller than 2^{-24} (which is the case for ~5% of weight gradient values in the paper)) and 2) that the ratio between weight value to weight update is large so that an update could become zero when aligning the binary point with the weight.

The authors solved these problems by 1) maintaining a master copy of weights in FP32 and 2) by loss-scaling.

The weights of the model are still maintained in FP32. In each iteration a copy of the master weights is created for which the values’ precision is halved. This copy is used to compute activations and gradients. At the end of an iteration the FP32 weights are updated with the FP16 gradients.
Creating a copy of the weights may seem to be contradictory since now we have increased the memory consumption by 50%. However, the main memory consumption comes from storing activations as these have to be saved during the forward pass so that they can be reused in the back-propagation. Because memory is saved for those FP16 values, the overall memory consumption is roughly halved.

By using loss-scaling the authors avoid gradient values from becoming zero. The loss value that is computed in the forward pass is scaled (e.g. by simply multiplying it with a constant which can be quite large as long as it does not cause overflows) prior to starting back-propagation. Before FP32 weights are updated the gradients are unscaled to maintain the magnitude of updates the same as in FP32 training.

In summary the authors have developed a very promising technique to train quite complex and popular models using only half precision floating point numbers. They have trained several models for language modeling, machine translation, image classification and speech recognition and in all of these cases they were able to match the accuracy of baseline FP32 models with the same hyper-parameters. The technique can be applied for very large models (e.g. with more than 100 million parameters) and the memory consumption was reduced by nearly 2x.
