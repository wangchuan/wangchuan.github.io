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

### Restore pre-trained models
In TensorFlow, a `tf.train.Saver` is a module to save and restore variables to disk. If we need to specify which variables to restore, we need to pass a variable `dict` or `list` to `tf.train.Saver`. Using `list` is simple, now we just talk about how to use `dict`.

```python
name_to_vars = { ... } # key: the variable name saved in the checkpoint file, value: the TensorFlow variable we need to restore.
saver_for_restore = tf.train.Saver(name_to_vars)
```

Note that in `name_to_vars`, the `key` is a string, i.e. the variable name saved in the checkpoint file, not the name you defined in your graph. The `value` is the TensorFlow variable we need to restore, it is the variable you define in your current graph. See the following code:

```python
def save_ckpt():
    FLAGS.mode = 'train'
    net = CENet3D(FLAGS)

    name_to_vars = {}
    for var in net.all_vars:
        if 'discriminator_im' in var.op.name:
            name = var.op.name.replace('discriminator_im', 'discriminator')
            name_to_vars[name] = var
        if 'generator_vd' in var.op.name or 'discriminator_vd' in var.op.name:
            name = var.op.name
            name_to_vars[name] = var
    saver_only_for_restore = tf.train.Saver(name_to_vars)
    saver_for_save = tf.train.Saver(net.all_vars)
    with tf.Session(config=tfconfig) as sess:
        init_op = tf.group(tf.global_variables_initializer(), tf.local_variables_initializer())
        sess.run(init_op)
        ckpt = tf.train.get_checkpoint_state(FLAGS.log_path)
        if ckpt and ckpt.model_checkpoint_path:
            saver_only_for_restore.restore(sess, ckpt.model_checkpoint_path)
            print('model restored...')
            saver_for_save.save(sess, os.path.join('./', 'initial_weights.ckpt'))
        else:
            raise ValueError('no model found!')
```

