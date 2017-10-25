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

<h1 style="text-align:center;">Blood Cell Classification via CNN and Hard Negative Mining</h1>

---

<p style="text-align:center;">
	<a href="http://i.cs.hku.hk/~cwang/">Chuan Wang</a><sup>1</sup>
</p>

<p style="text-align:center;">
	<sup>1</sup>Lenovo Group Limited, Hong Kong
</p>

<p style="text-align:center; margin-top: 30px;">
	<img src="cells.jpg" alt="demo" style="width: 40%;">
	<img src="prec-recall-confusion-matrix.png" alt="demo" style="width: 51%;">
</p>

<p style="margin-bottom: 30px; text-align:center;" markdown="1">
	Figure: Left: The blood cell image samples in 19 classes, provided by our customer. Right: The precision and recall for each class and the confusion matrix based on our training result. Note: we just release a result of around accuracy 86%, while the final solution released to our customer is over 90%.
</p>

### Abstract
<p style="text-align: justify; text-justify: inter-word;" markdown="1">
	We applied neural network to solve blood cell classification problem, based on data provided by Lenovo's external customer, a famous medical device manufacturer in China. The blood cells are in 19 classes, most of which share similar appearance. So we first developed a CNN-based classification network for it, and then applied Hard Negative Mining to tackle classes with low accuracies. The final solution released to our customer achieves over 90% accuracy on average, and will be directly embedded into our customer's products.
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
