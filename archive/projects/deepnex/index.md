---
layout: page
title: ""
tagline : ""
use_math: true
lang: zh
---
{% include JB/setup %}

{% assign posts_collate = site.categories.projects %}
{% include JB/posts_collate %}

<link rel="stylesheet" href="/glyphicons/css/glyphicons.css">

<h1 style="text-align:center;">Lenovo DeepNEX: A Distributed Multi-tenant Private Cloud for Deep Learning Development</h1>

---

<p style="text-align:center;">
	<a href="http://i.cs.hku.hk/~cwang/">Chuan Wang</a><sup>1</sup>
	&nbsp;&nbsp;&nbsp;&nbsp;
	Yaoman Li<sup>1</sup>
	&nbsp;&nbsp;&nbsp;&nbsp;
	Min Wang<sup>1</sup>
	&nbsp;&nbsp;&nbsp;&nbsp;
	<a href="http://www.jiachen.org/">Jia Chen</a><sup>1</sup>
</p>

<p style="text-align:center;">
	<sup>1</sup>Lenovo Group Limited, Hong Kong
</p>

<p style="text-align:center; margin-top: 30px;">
	<img src="main.jpg" alt="demo" style="width: 100%;">
</p>

<p style="margin-bottom: 30px; text-align:center;" markdown="1">
	Figure: (a) The dashboard view of DeepNEX, showing how many containers the user created. (b) Run Caffe command line in Ternimal. (c) Run TensorFlow in Jupyter Notebook. (d) System resource monitor view.
</p>

### Abstract
<p style="text-align: justify; text-justify: inter-word;" markdown="1">
	We developed a distributed multi-tenant private cloud platform for users who are interested in deep learning development. Our system runs in a cluster and supports multiple users to develop and run deep learning programs simultaneously. In our system, GPUs, CPUs and Memory can be well assigned by administrator in a web interface. And our system supports multiple mainstream toolkits like Caffe, TensorFlow, MXNet. Users can directly call these toolkits without any boring configurations.
	To support the development and sales of our system, I refactored various deep learning projects for demonstration and easy usage to our customers. These demo code include: image / text sentiment classification, image-to-image translation by CGAN, image recolorization by autoencoder etc., during which I also leart the related techniques.
</p>

---

<!--<table style="width:100%">
<col width="20%">
<col width="10">
<col >

</table>-->

<style type="text/css">
td {
    border: 0.5px;
    vertical-align: center;
    text-align: left;
}
</style>
