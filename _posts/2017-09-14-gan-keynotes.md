---
layout: post
title:  "Generative Adversarial Nets"
categories: coding
tags: tensorflow
---

* TOC
{:toc}

### The Original Version
Refer to Goodfellow's [paper](https://arxiv.org/pdf/1406.2661.pdf).
#### The Problem
We have a dataset, represented as $\{\vec{x}\}$, the data follows an unknown distribution $p_g$, now we would like to generate new data that looks identical to $\vec{x}$. The idea is as follows:

1. Define a random noise $\vec{z}$ following distribution $p_z(z)$, and let it pass a "Generator" $G(z;\theta_g)$, where $\theta_g$ is the parameters of $G$ ($G$ is differentiable). So we will get an output saying $G(z)$. It is a fake data.
2. Let the fake data $G(z)$ and real data $x$ go to a "Discriminator" $D$. $D$ will output a probability of the given input belonging to the **real** data. So ideally, for $D(x)$, its value should be as high as 1, but for $D(G(z))$, it should be as low as 0.
3. However, we hope to have such a $G$, it produces $G(z)$ which is fake but looks very very similar to $x$ itself, so that $D$ cannot distinguish from it from real data. In mathematical words, now $D(G(z))$ will become high as well.
4. But at the same time, although $G$ is strong enough, and $G(z)$ is very very similar to $x$, the little difference between $x$ and $G(z)$ still exists. We hope $D$ is also strong enough to distinguish such little difference.

So we have such minimax problem:

$$
\min_G \max_D V(G, D) = \mathbf{E}_{x\sim p_{data}(x)}[\log D(x)] + \mathbf{E}_{z\sim p_z(z)}[\log(1 - D(G(z)))]
$$

Let's look into the equation above carefully. Now we have two kinds of data, real data and fake data.
- Case 1: the data is real
 We hope to find a $D$ so that $D(x)$ is large, so $\log D(x)$ is large as well, i.e. $\max_D$
- Case 2: the data is fake
 We hope to find a $G$ so that $G(z)$ is similar to $x$, and it can fool $D$ as much as possible. So $D(G(z))$ will be large as well, so $\log(1 - D(G(z)))$ will be small. That is $\min_G$. At the same time, we also hope $D$ is strong enough so that it cannot be fooled, so that $D(G(z))$ is still low, i.e. $\log(1 - D(G(z)))$ will be large. i.e. $\max_D$.

At the end, $G$ and $D$ will be both strong enough.

Here, we need to indicate that, if we add a negative sign to the above problem, it becomes:

$$
\max_G \min_D V(G, D) = \mathbf{E}_{x\sim p_{data}(x)}[-\log D(x)] + \mathbf{E}_{z\sim p_z(z)}[-\log(1 - D(G(z)))]
$$

We can define two energy functions:

$$
\begin{align*}
E_1(D) &= \mathbf{E}_{x\sim p_{data}(x)}[-\log D(x)] + \mathbf{E}_{z\sim p_z(z)}[-\log(1 - D(G(z)))] \\
E_2(G) &= \mathbf{E}_{z\sim p_z(z)}[-\log(1 - D(G(z)))]
\end{align*}
$$

We switch between minimizing $E_1$ and maximizing $E_2$ to optimize the overall energy $V(G,D)$. We can also modify $E_2$ a little bit so that

$$
E_2(G) = \mathbf{E}_{z\sim p_z(z)}[-\log(D(G(z)))]
$$

which makes "maximizing" $E_2$ to "minimizing" $E_2$. 

#### TensorFlow Implementation
In TensorFlow, `tf.nn.sigmoid_cross_entropy_with_logits(x, z)` calculates like this:

$$
 z * -\log(\text{sigmoid}(x)) + (1 - z) * -\log(1 - \text{sigmoid}(x))
$$

Sigmoid function produces a result between 0 and 1, which meets the requirement that $D(x)$ also outputs such range. So normally we have such code:

```python
fake_data = generator(z)
real_data = ...

D_logits_real = discriminator(real_data) # not probability yet, only digits, scalar
D_logits_fake = discriminator(fake_data) # not probability yet, only digits, scalar

f = tf.nn.sigmoid_cross_entropy_with_logits

# -log(sigmoid(x)): 
d_loss_real = tf.reduce_mean(f(logits=D_logits_real, labels=tf.ones_like(D_logits_real))) 

# -log(1 - sigmoid( g(z)'s digit via discriminator ))
d_loss_fake = tf.reduce_mean(f(logits=D_logits_fake, labels=tf.zeros_like(D_logits_fake))) 

# -log(sigmoid( g(z)'s digit via discriminator ))
g_loss = tf.reduce_mean(f(logits=D_logits_fake, labels=tf.ones_like(D_logits_fake))) 

d_loss = d_loss_real + d_loss_fake

t_vars = tf.trainable_variables()

self.d_vars = [var for var in t_vars if 'd_' in var.name]
self.g_vars = [var for var in t_vars if 'g_' in var.name]
        
d_optim = tf.train.AdamOptimizer(args.lr, beta1=args.beta1).minimize(self.d_loss, var_list=self.d_vars)
g_optim = tf.train.AdamOptimizer(args.lr, beta1=args.beta1).minimize(self.g_loss, var_list=self.g_vars)
```