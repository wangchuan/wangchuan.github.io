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

<h1 style="text-align:center;">Content-Aware Unsupervised Deep Homography Estimation</h1>

---

<p style="text-align:center;">
	Jirong Zhang<sup>1,2,*</sup>&nbsp;
	<a href="/index.html">Chuan Wang</a><sup>2,*</sup>&nbsp;
	<a href="http://www.liushuaicheng.org/">Shuaicheng Liu</a><sup>1,2,#</sup>&nbsp;
	Lanpeng Jia<sup>2</sup>&nbsp;
	Nianjin Ye<sup>2</sup>&nbsp;
	<a href="http://www.juew.org/">Jue Wang</a><sup>2</sup>&nbsp;
	Ji Zhou<sup>1</sup>&nbsp;
	<a href="http://www.jiansun.org/">Jian Sun</a><sup>2</sup>&nbsp;
</p>

<p style="text-align:center;">
	<sup>1</sup>University of Electronic Science and Technology of China&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
	<sup>2</sup>Megvii Technology
</p>

<p style="text-align:center;">
	<sup>*</sup>Joint First Authors&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
	<sup>#</sup>Corresponding Author
</p>

<p style="text-align:center;">
	<i>The 16th AAAI European Conference on Computer Vision (ECCV 2020)</i>
</p>

<p style="text-align:center;">
	<i>arXiv <a href="https://arxiv.org/pdf/1909.05983.pdf">https://arxiv.org/pdf/1909.05983.pdf</a></i>
</p>

<p style="margin-bottom: 30px; text-align:center;">
	<img src="main.jpg" alt="deephomo" style="width:100%">
	Our deep homography estimation on challenging cases, compared with one traditional feature-based, i.e. SIFT + RANSAC and one unsupervised DNN-based method. (a) An example with dominate moving foreground. (b) A low texture example. (c) A low light example. We mix the blue and green channels of the warped image and the red channel of the target image to obtain the visualization results as above, where the misaligned pixels appear as red or green ghosts. The same visualization method is applied for the rest of this paper.
</p>

### Abstract
<p style="text-align: justify;
    text-justify: inter-word;">
  Homography estimation is a basic image alignment method in many applications. It is usually conducted by extracting and matching sparse feature points, which are error-prone in low-light and low-texture images. On the other hand, previous deep homography approaches use either synthetic images for supervised learning or aerial images for unsupervised learning, both ignoring the importance of handling depth disparities and moving objects in real world applications. To overcome these problems, in this work we propose an unsupervised deep homography method with a new architecture design. In the spirit of the RANSAC procedure in traditional methods, we specifically learn an outlier mask to only select reliable regions for homography estimation. We calculate loss with respect to our learned deep features instead of directly comparing image content as did previously. To achieve the unsupervised training, we also formulate a novel triplet loss customized for our network. We verify our method by conducting comprehensive comparisons on a new dataset that covers a wide range of scenes with varying degrees of difficulties for the task. Experimental results reveal that our method outperforms the state-of-the-art including deep solutions and feature-based solutions.
</p>

---

### Downloads
<table style="width:600px">
<tr>
<td markdown="1">

||<em class="icon-file"/>||[paper](paper.pdf)||

</td> 
</tr>

<tr>
<td markdown="1">

||<em class="icon-github"/>||<a href="https://github.com/JirongZhang/DeepHomography">source code</a>||

</td> 
</tr>

<tr>
<td markdown="1">

||<em class="icon-keynote"/>||coming soon||

</td> 
</tr>

</table>

---



### Bibtex


```bibtex
@article{zhang2019content,
  title={Content-Aware Unsupervised Deep Homography Estimation},
  author={Zhang, Jirong and Wang, Chuan and Liu, Shuaicheng and Jia, Lanpeng and Wang, Jue and Zhou, Ji},
  journal={arXiv preprint arXiv:1909.05983},
  year={2019}
}
```

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
