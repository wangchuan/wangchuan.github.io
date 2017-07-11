---
layout: post
title:  "TensorFlow APIs"
categories: coding
tags: tensorflow
---

* TOC
{:toc}

### [`tf.nn.softmax`](https://www.tensorflow.org/versions/master/api_docs/python/tf/nn/softmax)
在最后一个维度做softmax操作, 比如:

```python
import tensorflow as tf
import numpy as np

N = 10
C = 2
a = np.random.randn(N, C, C*4) * 100

aa = tf.constant(a)
bb = tf.nn.softmax(aa)

with tf.Session() as ss:
	bbval = ss.run(bb)
	print(bbval)
	print(np.sum(bbval, axis=2)) # <== will get all ones' result
```



