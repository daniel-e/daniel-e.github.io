---
layout: post
title: Summary&#58; In-Datacenter Performance Analysis of a Tensor Processing Unit
tags: [paper, machine learning]
---

# Summary: In Datacenter Performance Analysis of a Tensor Processing Unit

In 2013 Google started to discuss and develop an ASIC called Tensor Processing Unit (TPU) and started to deploy it in 2015 in its datacenters as a coprocessor on the PCIe I/O bus into Google’s existing servers just as it can be done with a GPU. The heart of the TPU is a 65,536 8-bit MAC matrix multiply unit that offers a peak throughput of 92 TeraOps/s. 

The need for a TPU became clear in 2013. At that time it was estimated that people using voice search for just three minutes a day Google would require its datacenters to double to meet the computation demands. 

The TPU was designed and optimized for inference (prediction) apps which usually emphasize response-time over throughput. GPUs like the K80 are usually underutilized for inference due to their high latency. For inference it is often good enough to use quantization. i.e. floating-point numbers are transformed into narrow integers - often just 8-bit integers. With quantization a 8-bit integer multiplication can be done with 6x less energy and it can be implemented using 6x less area on a die than 16-bit floating-point multiplications. The advantage of an integer addition is even 13x in energy and 38x in area. 

At Google only 5% of the neural network workload comes from CNNs. The rest of the workload comes mainly from MLPs and LSTMs. Hence, it was optimized for these architectures. While the K80 may excel at training, on average it is just a little faster than Haswell at inference.

The TPU is a big success story at Google. Compared to an Nvidia K80 GPU (eight K80 dies) and an 18-core Intel Haswell CPU the TPU is on average about 15x - 30x faster at inference with about 30x - 80x more TOPS/Watt. The success came from quantization, the low latency for inference apps, the big matrix multiply unit and the ability to run whole inference models (i.e. upload the model once and run it many times) to reduce dependence on host CPU. 

[paper](https://arxiv.org/pdf/1704.04760.pdf)
