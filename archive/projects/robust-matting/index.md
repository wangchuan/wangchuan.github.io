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

<h1 style="text-align:center;">An Implementation of Robust Matting Algorithm</h1>

---

<p style="text-align:center;">
	<a href="http://i.cs.hku.hk/~cwang/">Chuan Wang</a>
</p>

<p style="text-align:center;">
	The University of Hong Kong, Hong Kong
</p>

<p style="text-align:center;">
	<img src="main.jpg" alt="robustmatting" style="width:90%;">
</p>

<p style="margin-bottom: 30px; text-align:center;">
	Figure: Row 1: Original images. Row 2: Trimaps. Row 3: The computed matting results.
</p>

### Abstract
<p style="text-align: justify; text-justify: inter-word;">
	Back to 2011, when I first started my Ph.D in HKU, my training project was to implement such a robust matting algorithm, based on Dr. Jue Wang's paper "Optimized Color Sampling for Robust Matting". From this project, I learnt the usages of libraries like OpenCV, and I also learnt to write my own sparse matrix library which supports dynamic entry update and feeding into Intel MKL to use its multithreading linear solver. Recently I sorted out the code and applied Eigen to solve the linear system by its built-in Conjugate Gradient method.
</p>

---

### Code
<table style="width:600px">

<tr>
<td markdown="1">

||<em class="icon-github"/>||[Code](https://github.com/wangchuan/RobustMatting)||

</td> 
</tr>

</table>

---

### Reference
- Jue Wang, Michael F. Cohen, "Optimized Color Sampling for Robust Matting", *IEEE Conference on Computer Vision and Pattern Recognition, 2007*.


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
