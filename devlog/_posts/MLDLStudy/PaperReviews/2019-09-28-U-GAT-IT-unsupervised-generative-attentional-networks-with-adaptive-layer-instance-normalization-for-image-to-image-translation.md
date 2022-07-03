---
layout: post
title: (PaperReview) U-GAT-IT: unsupervised generative attentional networks with adaptive layer-instance normalization for image-to-image translation
tags: [PaperReview, DL, CV, img2img translation, GAN, AdaLIN]
categories: [MLDLStudy]
comments: true
sitemap: true
image: /assets/img/devlog/MLDLStudy/PaperReview/ugatit/paper-reviewugatit-unsupervised-generative-attentional-networks-with-adaptive-layerinstance-normalization-for-imagetoimage-translation-1-638.jpg
accent_image: 
  background: url('/assets/img/sidebar-bg.gif') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  도메인 간의 매핑 함수를 학습해 신기한 이미지를 만들고자 하는 Image to Image translation 분야의 새로운 시도가 계속되고 있는 가운데 NCSOFT의 김준호님이 1저자로 참여한 U-GAT-IT은 AdaLIN이라는 새로운 정규화 기법을 제안하고 CAM(Class Activation Map)과 Attention 구조의 적용으로 도메인에 따라서 모델의 구조 변경이나 하이퍼파라미터 변경 없이도 유연한 shape 및 texture 변형이 가능케 하는 새로운 방법을 소개합니다.
related_posts:
    - /devlog/_posts/Event&Seminar/2019-02-23-NAVERVisionAIHack.md
---
<center>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/FdV0SMnys08Em0" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> 
</center>
<Blockquote><span style="font-size:11pt">- review date 1: 2019/08/09 (by Meyong-Gyu.LEE @Soongsil Univ.)<br>- review date 2: 2019/08/11 (by Meyong-Gyu.LEE @Deeperence)<br>- Kor review of 'U-GAT-IT: unsupervised generative attentional networks with adaptive layer-instance normalization for image-to-image translation' (arXiv, Submitted on 25 Jul 2019)</span></Blockquote><br>

도메인 간의 매핑 함수를 학습해 신기한 이미지를 만들고자 하는 Image to Image translation 분야의 새로운 시도가 계속되고 있는 가운데 NCSOFT의 김준호님이 1저자로 참여한 U-GAT-IT은 AdaLIN(Adaptive Layer Instance Normalization)이라는 새로운 정규화 기법을 제안하고 CAM(Class Activation Map)과 Attention 구조의 적용으로 도메인에 따라서 모델의 구조 변경이나 하이퍼파라미터 변경 없이도 유연한 shape 및 texture 변형이 가능케 하는 새로운 방법을 제안합니다.<br>