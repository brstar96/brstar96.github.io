---
title: "Reconstruction of Monte Carlo Image Sequences using a Recurrent Denoising Autoencoder"
tags: 
  - Deep Learning
  - Computer Graphics
  - Computer Vision
  - RealTime Rendering
  - RCNN
  - AutoEncoder
  - Denoising
categories:
  - PaperReview
toc: false
comments: 
  provider: "disqus"
  disqus:
    shortname: "https-brstar96-github-io"
use_math: true
header:
  teaser: /assets/Images/paper-review-reconstruction-of-monte-carlo-image-sequences-using-a-recurrent-denoising-autoencoder-1-638
---
<center>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/89I0NjlftGP96j" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
</center><br>

<Blockquote><span style="font-size:11pt">- review date: 2019/07/26 (by Meyong-Gyu.LEE @Soongsil Univ.)<br>- Eng+Kor review of 'Reconstruction of Monte Carlo Image Sequences using a Recurrent Denoising Autoencoder' (Siggraph 2017)</span></Blockquote>

<span style="font-size:11pt">
Monte Carlo Rendering에 필연적으로 존재하는 noise를 denoising하기 위한 연구가 지금까지 계속되어 왔지만, Temporal artifact를 비롯한 다양한 문제들로 Offline rendering에 비해 썩 좋지 않은 결과물을 보여 왔습니다. NVIDIA의 연구팀은 Denoising AutoEncoder에 Recurrent connection 구조를 적용하여 Real-time Monte Carlo Rendering Sequence의 노이즈 제거에 효과적임을 입증합니다.<br><br>
누구나 생각해볼 법한 아이디어인것 같은데 왜 비슷한 연구가 많이 없을까 궁금했는데, 논문 말미에 그 답이 있었습니다. 훈련에 사용된 장비가 NVIDIA DXG-1이라는, 상당히 비싼 장비여서 진입장벽이 좀 있던 것 같습니다. Inference에 사용되는 장비도 Titan X로, 논문이 나온 2017년에는 꽤 고사양 장비였죠. 그나저나 2017년이면 2년 좀 안된 시기인데 옛날 논문처럼 느껴지는걸 보니 이쪽 분야의 발전이 정말 빨라도 너무 빠른 것 같습니다. 개인이 공부하는 속도를 이미 월등히 넘어서 버렸네요. <br>
</span>