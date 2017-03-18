---
layout: post
title: "3D Vision Releated Concepts"
categories: hidden
tags: special
use_math: true
---

* TOC
{:toc}

### Structure from Motion
#### What is SfM? 
Actually the phrase structure from motion is not that accurate for its basic meaning. In my viewpoint, structure **and** motion is more accurate. The process is that, given several images of a scene in multiple views, the algorithm will reconstruct the 3D geometry information of the scene as well as the camera poses of each image. The 3D geometry information of the scene include the point cloud of the feature points, that is the "structure"; The camera poses are actually the "motion" here.

#### Algorithm
Input: Multi-view images of the same scene \\
Output: the 3D points of features and Cameras' pose, i.e. $R$ and $t$.

1. Feature detection and correspondence
2. Feature points correspondence can derive the fundamental matrix, $x_1^T F x_2 = 0$, where $x$ is the image coordinate. In OpenCV, there is a built-in API
```cpp
Matx33d Fmat = findFundamentalMat(Points2DinCam0, Points2DinCam1, CV_FM_RANSAC);
```
3. Estimate essential matrix $E$, $E = K_1^T F K_2$, where $K$ is the intrinsic matrix of the camera.
4. $E = [t]_\times R$ => $R$ and $t$. Four possible solutions, only one is okay. (skew-symmetric matrix of a vector $t$, is the matrix representation of the cross product with $t$)
5. Triangulation. (back-projection, minimize the re-projection error with the 3D positions of the 3D points.)
6. After all the views done, bundle adjustment ([sba : A Generic Sparse Bundle Adjustment C/C++ Package Based on the Levenberg-Marquardt Algorithm](http://users.ics.forth.gr/~lourakis/sba/)). Non-linear optimization.


---

