---
layout: post
title: (PaperReview) Towards Foveated Rendering for Gaze-Tracked Virtual Reality
tags: [PaperReview, CG, Realtime Rendering, VR]
categories: [MLDLStudy]
comments: true
sitemap: true
image: /assets/img/devlog/MLDLStudy/PaperReview/towards-foveated-rendering-for-gaze-tracked-virtual-reality/paper-reviewtowards-foveated-rendering-for-gaze-tracked-virtual-reality-1-638.jpg
accent_image: 
  background: url('/assets/img/sidebar-bg.gif') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  본 논문은 VR환경에서 렌더링 연산량 절감을 시도하는 과정에서 함께 발생하는 알리아싱 문제를 해결하고자 합니다. Non-foveated rendering 화상과 유사한 품질을 가진 결과를 VR 환경에서 도출하고자 하고, 이를 위해 다양한 foveation 기술을 실험할 수 있는 샌드박스를 만들고 테스트하며 기존 foveated renderer의 개선 방안을 모색했습니다. 
related_posts:
    - /devlog/_posts/Event&Seminar/2019-02-23-NAVERVisionAIHack.md
---
<center>
<iframe src="//www.slideshare.net/slideshow/embed_code/key/lpi663VMPlZncc" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> 
</center>
<Blockquote><span style="font-size:11pt">- review date: 2017/10/30 (by Meyong-Gyu.LEE @Soongsil Univ.)<br>- Korean review of 'Towards Foveated Rendering for Gaze-Tracked Virtual Reality'(A Patney et al.)</span></Blockquote><br>

 본 논문은 VR환경에서 렌더링 연산량 절감과 함께 발생하는 알리아싱 문제를 해결하고자 합니다. 그 결과 non-foveated rendering 화상과 유사한 품질을 가진 결과를 VR 환경에서 도출하고자 하고, 이를 위해 다양한 foveation 기술을 실험할 수 있는 샌드박스를 만들고 테스트하며 기존 foveated renderer의 개선 방안을 모색하고자 하였습니다. 하지만 Gaze tracker를 피실험자가 착용해야 하므로 기존 VR 하드웨어의 변형이 필연적이며, 가벼운 눈 깜박임으로 인해 시선 추적이 일시 중지되어 그에 따른 Artifacts가 발생할 수 있다는 한계가 있습니다.<br>