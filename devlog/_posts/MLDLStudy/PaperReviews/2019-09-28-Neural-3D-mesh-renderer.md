---
layout: post
title: (PaperReview) Neural 3D mesh renderer
tags: [PaperReview, ML, DL, CV, Differentiable Renderer, 3D Rendering, 3D Reconstruction]
categories: [MLDLStudy]
comments: true
sitemap: true
image: /assets/img/devlog/MLDLStudy/PaperReview/neural-3d-mesh-renderer/paper-reviewneural-3d-mesh-renderer-1-638.jpg
accent_image: 
  background: url('/assets/img/sidebar-bg.gif') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  렌더링 분야에 신경망을 접목하기 위한 시도가 몇년 전부터 이어져 오고 있지만, 3D object가 Projection을 통해 screen space로 넘어간 후엔 2D에서 아무리 loss를 구해도 3D object space까지 gradient를 보낼 수 없다는 근본적인 한계가 있었습니다. 본 논문에서는 Approximated gradient 방법을 제안하여 다른 논문들보다 정확하게 Gradient를 2D space에서 3D object space로 전달할 수 있다고 주장합니다.
related_posts:
    - /devlog/_posts/Event&Seminar/2019-02-23-NAVERVisionAIHack.md=
---
<center>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/20WjPN1QXx1dst" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>
</center>
<Blockquote><span style="font-size:11pt">- review date: 2019/06/28 (by Meyong-Gyu.LEE @Soongsil Univ.)<br>- Korean review of 'Neural 3D mesh renderer'(CVPR2018)</span></Blockquote>

렌더링 분야에 신경망을 접목하기 위한 시도가 몇년 전부터 이어져 오고 있습니다만 3D object가 Projection을 통해 screen space로 넘어간 후엔 2D에서 아무리 loss를 구해도 3D object space까지 gradient를 보낼 수 없었습니다. 하지만 3D 공간에 대해 직접적으로 gradient를 흘려줄 수 있게 되면 3D 공간정보에 접근할 수 있게 됨으로서 좀 더 다양한 시도를 할 수 있게 됩니다. 본 논문에서는 왜 기존의 3D renderer에 신경망을 통합하는 과정이 어려웠는지 언급하고 Approximated gradient 방법을 제안하여 기존 다른 논문들보다 정확하게 Gradient를 2D space에서 3D object space로 전달할 수 있다고 주장합니다. <br><br>
선형 결합의 연속으로 표현 가능한 대다수의 Computer vision 문제와 달리 3D 렌더러는 그래픽스 렌더링 파이프라인 중 래스터라이제이션 과정에 포함된 sampling 때문에 미분이 불가능하게 됩니다. 하지만 모니터는 픽셀이라는 특유의 구조로 화상을 표현하기 때문에 화면에 3D 그래픽을 이루는 기본 도형인 삼각형을 모니터에 출력하기 위해선 래스터라이제이션 과정이 필수적이죠. 단순히 3D space에서 2D space로의 projection은 선형결합이므로 미분이 가능하지만 래스터라이제이션은 이미지를 화면에 뿌리기 위해 샘플링 과정을 수행하고, 이로 인해 미분이 불가능한 문제가 있었습니다. <br><br>
논문의 아이디어를 주장하고 난 후엔 세 개의 어플리케이션을 제안함으로서 본인들의 아이디어가 실현 가능함을 증명합니다. 퀄리티 문제를 벗어나 3D 공간에서도 얼마든지 backpropagation이 가능함을 보여주었다는 것에서 의미가 크다고 할 수 있겠습니다. 논문 저자들은 3D resonctruction 어플리케이션 부분에서만 limitation을 이야기하고 있지만, 제 생각에 전반적으로 하이폴리곤 메쉬의 역전파 과정에서 다소 시간이 오래 걸릴 것으로 추정됩니다. 또한 이제 시작 단계이므로 당연한 이야기이겠지만 아직 다양한 3D 그래픽스 효과들을 적용하기에는 무리가 있을 것으로 보이며, 다양한 조명들이 추가되는 상황에서도 강건하게 작동할지 또한 미지수입니다. <br> 