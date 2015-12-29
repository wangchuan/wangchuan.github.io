## Wang Chuan's Homepage
### Dr. Chuan Wang

*Specialise in Computer Vision and Image/Video Processing*

[CV in pdf](http://wangchuan.github.io/cv/CV_ChuanWang.pdf)

#### Contact

- Phone: (+852) 5162 6756 (HK) / (+86) 131 4751 1986 (Mainland China)
- Email: wangchuan2400 (at) gmail (dot) com

#### Objectives

I am seeking a position in IT industry where I can contribute my proficient programming skills, energy, high motivation and dedication to the company. My research interests include image/video processing, 3D vision, deep learning and graphics. If you have interest in me, feel free to contact me.

#### Education

- Ph.D, Computer Science (Vision and Graphics), 2015, Supervised by [Prof. Wenping Wang](http://www.csis.hku.hk/people/profile.jsp?teacher=wenping), The University of Hong Kong (HKU).
- B.Eng, Electronic Information Engineering, 2010, University of Science and Technology of China (USTC).

#### Work and Research Experiences

**Staff Researcher, Lenovo. **(April. 2015 - Now)

- **An Augmented Reality Application for Mobile Devices**. ([Youtube](https://youtu.be/XUTCowMHSQs))
	- Developed an application providing augmented reality effect when camera previewing scenes containing QR code or a dish of food, to improve user experiences.
	- The application detects and tracks the features as well as estimates the camera poses by the 3D-2D perspective n-points correspondence algorithm. With the camera pose estimated for each frame, we render an virtual object by OpenGL for the scene.

![ARDemo](http://wangchuan.github.io/imgs/AR.gif)

- **An Image Refocus Algorithm for Dual-camera Cellphone of Lenovo**.
	- Developed and Improved an image refocus algorithm for Lenovo dual camera phone VIBE S1, which is on sale in the market.
	- The algorithm uses the depth information captured by the dual camera to blur the image with a spatially-variant kernel if users specify a focus point when viewing the image.
	- To achieve real-time interaction, we utilized OpenCL to speed up the blurring process. We also utilized line-buffer idea to save memory.

![refocus](http://wangchuan.github.io/imgs/refocus.jpg)

- **Speaker Identification via Deep Learning**.
	- Co-developed a speaker identification system based on Recurrent Neural Network (RNN), specifically Long Short-Term Memory (LSTM) technique.
	- The system utilizes both face image and audio information in a video sequence to classify the speaker. The novelity lies in such a multi-modal LSTM solution.
	- Did experiments to demonstrate this solution can achieve higher precision than state-of-the-art Convolution Neural Network (CNN) based method, implemented based on Caffe. The paper was accepted in AAAI 2016.

![aaai](http://wangchuan.github.io/imgs/aaai.jpg)

**Ph.D, The University of Hong Kong.** (Sep. 2010 - Jan. 2015)

- **Video Vectorization**.
	- Proposed a vector-based representation, i.e. tetrahedral mesh, for raster video to achieve low storage with resolution-independent high quality of reconstruction. Also developed a system to convert a raster video into its vectorized version.
	- Developed a core algorithm for the system, i.e. a tetrahedral mesh simplification algorithm extended from the one for triangular mesh but handles more complicated topological cases. Also implemented subdivision and deformation algorithm for the tetrahedral mesh to preserve the features in the original video.
	- Developed a rasterization method to convert the vectorized version back, i.e. 1) slice the tetrahedral mesh into triangular meshes; 2) render each triangular mesh as each frame by OpenGL.
	- Designed a streaming strategy to process a video to achieve low memory cost but keeps the topology of the corresponding tetrahedral mesh.

![vectorization](http://wangchuan.github.io/imgs/vectorization.png)

- **Video Object Co-Segmentation**. ([Project Page](http://i.cs.hku.hk/~cwang/videocoseg/))
	- Developed a common-foreground co-segmentation system for a group of videos automatically.
	- Proposed a novel customized subspace clustering algorithm to differentiate foreground and background within each video but correlate the common foreground in various videos. This algorithm takes advantage of appearance and motion features in the video group discriminatively, so as to outperform state-of-the-art methods.
	- To build up the system, utilized existing mature techniques like K-means algorithm, Gaussian Mixture Model, Bag-of-Words Model and Markov Random Field etc. The paper was published in TMM 2014.

![videocoseg](http://wangchuan.github.io/imgs/videocoseg.png)

- **Image Matting Application**. ([Project Page](https://bitbucket.org/wangchuan2400/robustmatting))
	- Developed an interactive application to cut foreground from an image based on Robust Image Matting algorithm.

![matting](http://wangchuan.github.io/imgs/matting.png)

#### Skills

- Proficient in Native C/C++, MATLAB.
- Proficient in OpenCV; Familiar with OpenGL; Experience of using OpenCL, CUDA, Boost, Intel MKL, CGAL etc.
- Proficient in developing with Visual Studio in Windows; Experience of developing in Linux.
- Familiar with using computer vision or machine learning related techniques such as CNN(Caffe), Optical/SIFT Flow, Image Deblurring, Calibration, Structure from Motion etc.
- Familiar with .NET Framework, C#, WPF, Managed C++.
- Familiar with GUI design in WPF or QT; Love designing applications with fancy GUI (e.g. here by C++/C#/WPF).
- Familiar with using Microsoft Office, Adobe Photoshop/Illustrator/Premiere, MeshLab.

#### Selected Publications

- **Chuan Wang**, Yanwen Guo, Jie Zhu, Linbo Wang, Wenping Wang. Video object co-segmentation via subspace clustering and quadratic pseudo-boolean optimization in an MRF framework. IEEE Transactions on Multimedia 2014, 16(4): 903-916.
- Jimmy SJ. Ren, Yongtao Hu, Yu-Wing Tai, **Chuan Wang**, Li Xu, Wenxiu Sun, Qiong Yan, "Look, Listen and Learn - A Multimodal LSTM for Speaker Identification", The 30th AAAI Conference on Artificial Intelligence (AAAI-16).

*Other TIP and TMM submissions are under review.*

--------
*Last Update: Dec. 15, 2015*