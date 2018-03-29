---
layout: post
title: Summary&#58; TFX - Machine Learning Platform at Google
tags: [paper, machine learning]
---
<div style="font-size:small; color: gray; font-style: italic">
  By <a href="https://twitter.com/dnl0x00">@dnl0x00</a>
</div>

# TFX - Machine Learning Platform at Google

Here’s a summary of a recently published [paper](http://stevenwhang.com/tfx_paper.pdf) in which the authors at Google present TFX (TensorFlow Extended) , a TensorFlow-Based Production-Scale Machine Learning Platform. It consists of several components: data analysis, data transformation, data validation, trainer, model evaluation and validation and serving. Each component implements high-level functionality that is typical in machine learning and one goal of these high-level functions is to ensure consistency across the pipeline, e.g. by ensuring that the same transformations are applied to training and serving.

Data analysis and understanding the data is a crucial component in machine learning. Faults (e.g. due to code bugs) could introduce anomalies in the data. The authors argue that such errors are not an exception, but more the norm. Hence, to detect anomalies the data analysis component gathers several statistics over the feature values (e.g. mean, stddev, quantiles, etc).

The data transformation component for example generates feature-to-integer mappings.

After data analysis and data transformation data validation is applied. For this the users of TFX can provide a schema which describes the expected properties of the data, e.g. which features should be present in the data, what should be the fraction of examples that must contain the feature and what is the expected range of a feature. In case of a detected anomaly TFX can also provide recommendations to fix the anomaly, e.g. marking a feature as deprecated so that it is not used for training in the future. In case anomalies correspond to a natural evolution of the data TFX could adapt automatically the schema to bring it up-to-date. Anomalies are filed like software bugs. They are documented, tracked and eventually resolved.

The model training component makes it easy to run TensorFlow code. It also supports warm-starting for use cases for which a fresh model is required but for which the datasets are too large and hence prevent building a model in a reasonable amount of time.

The model evaluation and validation component checks that the model is safe to serve (e.g. does not crash, does not use too many resources) and that the prediction quality is as expected. Also, metrics can be computed on slices of data, for example for on US customers only.

The serving component is a complete serving solution which is provided via TensorFlow Serving to deploy models in production environments.

From their experience on large-scale machine learning the authors also conclude that Apache Beam and similar internal infrastructure (which unfortunately is not described in the paper) fits well for handling large volume of data during training, model evaluation and batch inference.

Finally, the authors present a case study for the Google Play recommender engine which particularly benefits from the warm-starting feature. Instead of training from scratch which would take several days, Google can now “frequently” (what that means is not mentioned) publish a new high quality model. It also helped to discover a training-serving skew. By fixing this flaw the performance of the model was improved by 2%.
