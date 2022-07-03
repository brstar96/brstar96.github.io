---
layout: post
title: (PaperReview) Few-Shot Adversarial Learning of Realistic Neural Talking Head Models
tags: [ML, img2img translation, CV, Meta Learning, Few-Shot Learning, GAN]
categories: [MLDLStudy, PaperReview]
comments: true
sitemap: true
image: /assets/img/devlog/MLDLStudy/PaperReview/realistic-neural-talking-head-models/paper-reviewfewshot-adversarial-learning-of-realistic-neural-talking-head-models-1-638.jpg
accent_image: 
  background: url('/assets/img/sidebar-bg.gif') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  2019년 5월 삼성 모스크바 AI Research에서 arXiv에 퍼블리시한 퓨샷러닝 논문입니다. 단 한장의 이미지로 움직이는 talking head를 만들어주는 네트워크로, 메타 러닝을 퓨샷 러닝에 적용해 빠른 학습 시간을 자랑합니다.
related_posts:
    - /devlog/_posts/Event&Seminar/2019-02-23-NAVERVisionAIHack.md
---
<center>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/Fmgy6rzR1zfo2Y" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
</center>
<Blockquote><span style="font-size:11pt">- review date 1: 2019/08/23 (by Meyong-Gyu.LEE @Soongsil Univ.)<br>- review date 2: 2019/08/24 (by Meyong-Gyu.LEE @Deeperence)<br>- Kor review of 'Few-Shot Adversarial Learning of Realistic Neural Talking Head Models' (arXiv, Submitted on 20 May 2019)</span></Blockquote>

올해 5월 삼성 모스크바 AI Research에서 arXiv에 퍼블리시한 퓨샷러닝 논문입니다. 단 한장의 이미지로 움직이는 talking head를 만들어주는 네트워크로, 메타 러닝을 퓨샷 러닝에 적용해 빠른 학습 시간을 자랑합니다. 기존에도 talking head를 만들기 위한 다양한 기법들이 존재하였으나 대부분 거대한 데이터셋(ObamaNet의 경우 17시간 가량의 연설 음성이 필요했습니다.)이 필요하며 시간도 오래 걸린다는 단점이 존재했습니다. 또한 사용자별로 이러한 거대 데이터셋을 구축하기란 매우 어려운 문제이므로 메타 러닝에 대한 연구로 talking head 분야의 연구가 이어지는 것은 어찌보면 당연하다고 볼 수 있겠습니다. 노테이션이 복잡해 이틀을 꼬박 고생하며 읽었네요. <br>