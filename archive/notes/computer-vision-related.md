---
layout: post
title: "Computer Vision Releated Concepts"
categories: hidden
tags: special
use_math: true
---

* TOC
{:toc}

## 3D Vision
### Structure from Motion
#### What is SfM? 
Actually the phrase structure from motion is not that accurate for its basic meaning. In my viewpoint, structure **and** motion is more accurate. The process is that, given several images of a scene in multiple views, the algorithm will reconstruct the 3D geometry information of the scene as well as the camera poses of each image. The 3D geometry information of the scene include the point cloud of the feature points, that is the "structure"; The camera poses are actually the "motion" here.

#### Algorithm
Input: Multi-view images of the same scene \\
Output: the 3D points of features and Cameras' pose, i.e. $R$ and $t$.

> 1. Feature detection and correspondence
2. Feature points correspondence can derive the fundamental matrix, $x_1^T F x_2 = 0$, where $x$ is the image coordinate. In OpenCV, there is a built-in API
```cpp
Matx33d Fmat = findFundamentalMat(Points2DinCam0, Points2DinCam1, CV_FM_RANSAC);
```
3. Estimate essential matrix $E$, $E = K_1^T F K_2$, where $K$ is the intrinsic matrix of the camera.
4. $E = [t]_\times R$ => $R$ and $t$. Four possible solutions, only one is okay. (skew-symmetric matrix of a vector $t$, is the matrix representation of the cross product with $t$)
5. Triangulation. (back-projection, minimize the re-projection error with the 3D positions of the 3D points.)
6. After all the views done, bundle adjustment ([sba : A Generic Sparse Bundle Adjustment C/C++ Package Based on the Levenberg-Marquardt Algorithm](http://users.ics.forth.gr/~lourakis/sba/)). Non-linear optimization.

### Fundamental matrix
The fundamental matrix $F$ is $3\times3$, has 7 dof. \\
Why?
There are 9 matrix elements but only their ratio is significant, which leaves 8 dof. In addition the elements satisfy the constraint $\text{def}\ F = 0$ which leaves **7** dof.

Properties:

>1. $x^TFx = 0$, where $x$ is the pixel coordinate.
2. $\text{rank}(F) = 2$, i.e. $\text{det}(F) = 0$.
* 8-points algorithms. Same as Essential matrix's. (only use property 1)
* 7-points algorithms. Property 1 and 2 are involved.

### Essential matrix
The essential matrix $E$ is $3\times3$, it has five or six degrees of freedom, depending on whether or not it is seen as a projective element. \\
Why?
The rotation matrix $R$ and the translation vector $t$ have three degrees of freedom each, in total six. If the essential matrix is considered as a projective element, however, one degree of freedom related to scalar multiplication must be subtracted leaving five degrees of freedom in total.

Properties:

>1. $y^TEy = 0$, where $y$ is the normalized image coordinate. 
2. $\text{def}\ E = 0$
3. $2EE^TE - \text{tr}(EE^T)E = 0$ (This produces 3 equations, but only 2 independent)
	* [8-points algorithm](https://en.wikipedia.org/wiki/Eight-point_algorithm) (only use Property 1):
		1. Formulating a homogeneous linear equation; 
			<p align="center" markdown="1">![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/b357a2c6c63a8d06bfb4229a5680b3c449937241){:width="70%"}</p>
		2. Solving the equation; (total least squares problem)
		3. Enforcing the internal constraint. (find an optimal $E$ whose singular values follow that two of its singular values are equal and nonzero and the other is zero, and the difference between this optimal $E$ and the $E$ we computed the least.)
	* 5-points algorithm (use all properties):
	1. 5 points gives 5 equations of property 1
	2. property 2 is one constraint
	3. property 3 gives 9 equations, but only two of them are algebraically independent.

### SolvePnP
Perspective N Points algorithm is originally derived from the camera perspective model. In this model, the world coordinates of a point $X$ in 3D space and its 2D coordinates in the image plane follows the matrix-equation:

$$s[u_x, u_y, 1]^T = K [R\ |\  t] [X, Y, Z, 1]^T$$

1. In this equation, since we use homogeneous coordinates, it involves two number equations, or we say two constraints. At the same time, since $R$ has DoF of 3, and $t$ has DoF of 3, it means there are 6 variables in the equations. So if we have at least 3 such kind of matrix-equations (each one provides 2 constraints), we can solve this system. Of course, in practice, the number of points may be larger than 3, and we will formulate the problem as a minimization problem, that is to find a pair of $R, t$ such that the difference between left hand side and right hand side can be minimized.
2. At the same time, it is clear to see that in this minimization problem, the variables are not linear to the system, which means it is a non-linear optimization problem. And normally Levenberg-Marquardt iterative algorithm is a good tool to such kind of non-linear optimization problem.

### Homography
Homography is a transformation between two views of a plane. It is a $3\times3$ matrix up to a scale, so degrees of freedom is 8. It is applied to the homogeneous coordinates of 2D points. (4 points can calculate the result.)

The functions find and return the perspective transformation $H$ between the source and the destination planes:

$$s_i  [x'_i, y'_i, 1]^T \sim H  [x_i, y_i, 1]^T$$

so that the following back-projection error is minimized,

$$\sum _i \left ( x'_i- \frac{h_{11} x_i + h_{12} y_i + h_{13}}{h_{31} x_i + h_{32} y_i + h_{33}} \right )^2+ \left ( y'_i- \frac{h_{21} x_i + h_{22} y_i + h_{23}}{h_{31} x_i + h_{32} y_i + h_{33}} \right )^2$$

In this matrix-equation, it actually involves 2 number equations, but the DoF of $H$ is 8, so at least 4 point pairs can solve the problem. In practice, since the equations are not linear, the formulated problem is naturally a non-linear optimization problem, which could be further solved by Levenberg-Marquardt iterative algorithm.

## 2D Vision
### KLT Tracker (Kanade-Lucas-Tomasi)
KLT Tracker is an optical-flow like tracker, for sparse feature points. Given two frames `f1` and `f2`, of course there are some feature points in `f1`, the idea is to find a displacement vector for each feature point in `f1`, such that after moving along the displacement vector, the local image at `f2` are the most similar to `f1`. And this concept is very similar to optical flow, because Optical flow is actually the movement you view in the moving world, and in computer vision, if given 2 frames, the optical flow is the transition between the two frames. That is to say, there is a 2D vector defined at each feature point, representing the displacement of the feature point from frame 1 to frame 2.
Usually the methods to compute optical flow can be divided into 4 categories:

- Gradient-based
- Block Matching-based
- Energy Function-based
- Phase-based

While in OpenCV, as I know there have been several functions working on it, such as 

- `calcOpticalFlowPyrLK` (sparse) <em class="icon-thumbs-up"></em>
- `calcOpticalFlowFarneback` (dense)
- `CalcOpticalFlowBM` (block matching-based)
- `CalcOpticalFlowHS`
- `calcOpticalFlowSF`

The basic idea of LK is
<p align="center" markdown="1">
	![LK1](https://wikimedia.org/api/rest_v1/media/math/render/svg/749d10ea42b4fb5f9955342d8e294bed10b7c8fd){:width="50%"}H.O.T
</p>

So 
<p align="center" markdown="1">
	![LK2](https://wikimedia.org/api/rest_v1/media/math/render/svg/dcea639f5b077f0b62fb6b3e80cac328371a197b){:width="20%"}
</p>

How to read? *"The derivative of image $I$ in $x$ axis multiply with the displacement of $x$, plus the derivative of image $I$ in $y$ axis multiply with the displacement of $y$ equals to minus the derivative of image $I$ in time axis."*

Usually we assume multiple pixels follow the same $V_x$ and $V_y$, thus with multiple pixels, we have multiple equations making the aforementioned an over-determined equation system, which can be solved by least square method.
<p align="center" markdown="1">
	![LK3](https://wikimedia.org/api/rest_v1/media/math/render/svg/4dba1d119546e8fe15ad701c99d0e32595f9a6c8){: width="50%"}
</p>
Then $A^TAv = A^Tb$ so we get 
<p align="center" markdown="1">
	![LK5](https://wikimedia.org/api/rest_v1/media/math/render/svg/7d94a7e72c2c1f8bda30c925e57d543cb4d48145){:width="50%"}
</p>
---

