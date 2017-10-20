---
layout: post
title: CPU vs GPU for Machine Learning
tags: [machine learning]
---

_In this article I just summarize some thoughts while reading an article about machine learning doing on a CPU vs GPU. All of my statements are not well researched and I could be totally wrong. Keep that in mind when you're reading this post. ;)_

Recently I have found an interesting [article](https://www.nextplatform.com/2017/10/13/new-optimizations-improve-deep-learning-frameworks-cpus/) in which the author stated that when using the Intel Xeon SP processor family for machine learning and CPU optimized libraries “the top performance gap is closer to 2X than it is to 100X”. I agree with the article that when using Intel processors you are more flexible and gain from other advantages (have a look at the well written article) but I don’t agree with the performance gap and especially if you consider the costs that are required to close the performance gap.

For example, when I search for the Intel Xeon Platinum 8180M processor, a processor with 28 cores and 56 threads, you get results where one CPU costs about 13,000€ (see [here](https://www.future-x.de/intel-xeon-platinum-8180m-25-ghz-28-core-56-threads-385-mb-cache-speicher-lga3647-socket-oem-p-677218/?pv=24&gclid=Cj0KCQjw95vPBRDVARIsAKvPd3L4yf2KabWImppwwCiul9zdr19nYhPL9Qj4YtLR7Xn4xE4XrJ-ZF10aArWvEALw_wcB)). This processor achieves about 1.5 Linpack TFLOPS (see [here](https://www.top500.org/news/intel-forges-new-xeon-line-under-scalable-processor-banner/)).

On the other hand a GeForce GTX 1080 achieves about 8.87 TFLOPS (single point) (see [table](https://www.heise.de/newsticker/meldung/Grafikkarte-Nvidia-GeForce-GTX-1080-Monster-Leistung-fuer-fast-800-Euro-3209254.html)) for a price of about 550€ at Amazon.

So, at the end, you get about 16 GFLOPS/€ when you buy a GeForce and about 0.12 GFLOPS/€ when you plan to use the 8180M processor. The gap (performance/€) is about 140.

It’s possible that the Linpack benchmark cannot be compared with the GeForce benchmark. Maybe I've compared apples and oranges. Maybe the Linpack benchmark measures double precision floating point (haven’t checked this). Nevertheless, even if the GeForce achieves only 4 TFLOPS double precision the gap is still >50X, not 2X.

