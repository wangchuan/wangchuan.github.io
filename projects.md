---
layout: page
title: "Main Projects"
tagline : ""
use_math: true
lang: zh
---
{% include JB/setup %}

{% assign posts_collate = site.categories.projects %}
{% include JB/posts_collate %}

--- 

<link rel="stylesheet" href="/glyphicons/css/glyphicons.css" />

<table style="width:100%">
<col width="20%">
<col width="10">
<col >

<tr style="border-bottom:1pt solid #eee">
<td markdown="1">
![deepnex](/img/main/deepnex.png){:class="img-shadow"}
</td>
<td></td>
<td markdown="1">
[**Lenovo DeepNEX: A Distributed Multi-tenant Private Cloud for Deep Learning Development**](/archive/projects/deepnex)
- Integrated deep learning toolkits e.g. Caffe, TensorFlow etc. into Docker container for use in the cloud.
- Refactored deep learning projects for demonstration to our customers.

|| <em class="icon-home"/> || [project page](/archive/projects/deepnex) ||

</td> 
</tr>

<tr style="border-bottom:1pt solid #eee">
<td markdown="1">
![captcha](/img/main/captcha.jpg){:class="img-shadow"}
</td>
<td></td>
<td markdown="1">
[**CAPTCHA Cracking: A CNN Based OCR Module for Web Crawler.**](/archive/projects/captcha-crack)
- Extended single-label classification to multi-label classification in CNN, and applied it into text recognition in a single image as CAPTCHA.
- An end-to-end solution which avoids image segmentation for a single character.
- Augmented synthesized data, to avoid overfitting when model applied to real data.

|| <em class="icon-home"/> || [project page](/archive/projects/captcha-crack) ||

</td> 
</tr>

<tr height="20"/>

<tr style="border-bottom:1pt solid #eee" >
<td markdown="1">
![3dcam](/img/main/3dcam.jpg){:class="img-shadow"}
</td>
<td></td>
<td markdown="1">
[**3D Camera: RGBD Image Algorithms for Dual-camera Mobile Phone, Lenovo VIBE S1.**](/archive/projects/3d-camera)
1. Image Refocus
- Developed an image refocus algorithm which utilizes the depth information to blur the image to simulate an
effect of "large aperture, shallow depth of field".
- Developed efficient algorithms and applied parallel computing (OpenCL) to achieve real-time interaction.
2. Image Matting
- Co-developed an automatic selfie image matting algorithm. Over-segmentation, region-wise matting and
parallel processing are involved to save memory and speed up the program.

|| <em class="icon-home"/> || [project page](/archive/projects/3d-camera) || <em class="icon-film"/> || [video demo](https://youtu.be/8gFGsBY3rzg) ||

</td> 
</tr>

<tr height="25"/>
<tr style="border-bottom:1pt solid #eee" >
<td markdown="1">
![arcam](/img/main/arcam.gif){:class="img-shadow"}
</td>
<td></td>
<td markdown="1">
**AR Camera: An Augmented Reality Prototype for Mobile Devices of Lenovo.**
- Prototyped an application with an AR effect for QR code or a dish of food, to improve user experience.
- Developed detection, tracking and stereo algorithms to obtain a real-time and smooth effect.

|| <em class="icon-film"/> || [video demo](https://youtu.be/XUTCowMHSQs) ||

</td> 
</tr>

<tr height="25"/>

<tr style="border-bottom:1pt solid #eee" >
<td markdown="1">
![rbmatting](/img/main/matting.jpg){:class="img-shadow"}
</td>
<td></td>
<td markdown="1">
[**Image Matting Application.**](/archive/projects/robust-matting/)
- Developed an interactive application to cut foreground from an image based on Robust Image Matting algorithm.

|| <em class="icon-home"/> || [project page](/archive/projects/robust-matting/) || <em class="icon-github"/> || [source code](https://github.com/wangchuan/RobustMatting) ||

</td>

</tr>

</table>

<style type="text/css">
td {
    border: 0.5px;
    vertical-align: center;
    text-align: left;
}
</style>
