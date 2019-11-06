---
title: "(PaperReview+Test) A Versatile Learning based 3D Temporal Tracker - Scalable, Robust, Online"
tags: 
  - Machine Learning
  - Random Forest
  - Computer Vision
  - AR
categories:
  - PaperReview
  - Shoveling
toc: false
author_profile: false
comments: 
  provider: "disqus"
  disqus:
    shortname: "https-brstar96-github-io"
use_math: true
header:
  teaser: /assets/Images/paper-reviewa-versatile-learning-based-3d-temporal-tracker-scalable-robust-online-1-638.jpg
---
<center>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/pA0qc4zWxED3JJ" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
</center><br>

<Blockquote><span style="font-size:11pt">- review date: 2018/04/09 (by Meyong-Gyu.LEE @Soongsil Univ.)<br>- Eng review of 'A versatile learning based 3D temporal tracker - scalable, robust, online'(ICCV 2015)</span></Blockquote>

<span style="font-size:11pt">
'A Versatile Learning based 3D Temporal Tracker - Scalable, Robust, Online' 논문은 642개의 정점을 가진 Geodesic Grid의 각 vertex에 Depth Camera를 배치하고 촬영하여 사전에 Depth Data와 그에 대응하는 Object Transformation의 pair들을 모은 후 Random Forest로 학습해 Tracking단계에서 Depth Image만으로 Real world의 Object가 가진 Transformation을 Prediction(Regression)하는 논문입니다. <br>
</span>

<iframe width="595" height="485" src="https://serviceapi.nmv.naver.com/flash/convertIframeTag.nhn?vid=3ED08D38E08D7597FD19539917A6A68D3809&outKey=V129a9b244ee0879762c96f850d20745d59cd0fde417e5af9a48e6f850d20745d59cd" frameborder="no" scrolling="no" title="NaverVideo" allow="autoplay; gyroscope; accelerometer; encrypted-media" allowfullscreen></iframe><br>

<span style="font-size:11pt">
연구실의 다른 선생님과 이 논문을 따라서 구현해보고 있었는데, 우선 전체 642개 중 1개의 뷰포인트에서 얻은 데이터로 학습시킨 결과는 위 영상과 같습니다. 빨간색 Ground Truth 토끼를 파랑색의 Predicted 토끼가 열심히 따라가려고 노력하고 있습니다. 하이퍼파라미터 튜닝도 덜 되었고, 학습 데이터도 많지 않은데다 논문의 Eq.3 구현도 안되어서 결과는 당연히 와장창이지만 loss 그래프를 뽑아 확인해 본 결과 학습의 방향은 얼추 맞게 진행되는걸 알 수 있었습니다.<br><br>
다른 네트워크를 실험해 보지는 않았지만 Depth Image를 통한 Prediction에는 Random Forest 모델이 가장 좋은 성능을 보여줍니다. 예시로, MS Kinect의 휴먼 스켈레톤 추출 알고리즘도 RF를 사용하고 있죠. 다만 Depth Data는 차원이 굉장히 높기 때문에, 본 논문에서 제안하는 것처럼 20개 정도의 front face 버텍스를 고르고 Displacement를 구해 학습시키는 것과 같이 고차원 데이터를 대신할만한 무언가로 학습시키는 것이 좋습니다. 일종의 Feature Engineering인 셈이죠.<br>
</span>