---
title: "(PaperReview) 3D shape reconstruction from sketches via multi view convolutional networks"
tags: 
  - Deep Learning
  - Computer Graphics
  - Computer Vision
  - Image to Image Translation
  - Sketch to 3D Model
  - 3D Reconstruction
categories:
  - PaperReview
toc: false
comments: 
  provider: "disqus"
  disqus:
    shortname: "https-brstar96-github-io"
use_math: true
header:
  teaser: /assets/Images/paper-review3d-shape-reconstruction-from-sketches-via-multi-view-convolutional-networks-1-638.jpg
---
<center>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/HRfU2axTgzNfhe" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
</center><br>

<Blockquote><span style="font-size:11pt">- review date: 2019/03/20 (by Meyong-Gyu.LEE @Soongsil Univ.)<br>- Korean review of '3D Shape Reconstruction from Sketches via Multi-view Convolutional Networks'(CVPR 2017)</span></Blockquote>

<span style="font-size:11pt">
3D shape reconstruction from sketches via multi view convolutional networks는 U-Net과 VanilaGAN을 활용해 스케치(Front, Side)로부터 3D Mesh를 reconstruction하는 연구입니다. 기존 연구들이 Voxel과 3D CNN 기반으로 진행되어 온 반면 본 논문은 스케치를 이용해 12개 뷰포인트에 대한 Depth map을 예측하고 그로부터 Normal map과 Mask를 추출하여 3D point cloud를 생성합니다. 이후 이 Point cloud로 screened Poisson Surface Reconstruction algorithm을 돌려 3D mesh로 변환 및 fine-tuning 과정을 거칩니다. 스케치로부터 Reconstruction 과정에 사용되는 Depth map이나 Normal map, Mask map과 같은 Information textures를 예측한다는 점에서 Sketch → Information textures로의 Image to image translation 문제로 보아도 되겠습니다. <br><br>
개인적인 감상으론 스케치를 이용해 3D mesh를 만드는 발상이 게임 및 3D 프린팅 분야에서 이상적으로 들릴 수 있겠으나 대다수의 GAN 및 Image to image translation 솔루션들이 그렇듯 아카데믹한 성격이 강하고 상업 목적으로 이용하기엔 한계가 큽니다. 디테일 묘사가 떨어진다는 점은 둘째치고라도 Information texture의 prediction에 대략 4.5초 정도밖에 걸리지 않지만 prediction을 얻기 위해 하나의 shape(비행기, 사람 등)에서 Titan X GPU 기준 2일(4K training sketches, batch size 2), RTX 2080TI 기준 약 일주일(batch size 1)이 학습 과정에 소요된다는 점, predicted information texture들로 3D mesh를 구성하는데 대략 4초가 소요되지만 Intel Xeon E5-2699 v3(약 350만원) CPU를 두 대나 사용한다는 점에서 상당히 비효율적이라고 할 수 있겠습니다. (I7-9700k는 40분 소요)<br>
</span>