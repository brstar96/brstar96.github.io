---
layout: project
title: Depth Map Guided Partial Super Resolution for Out-focused Video (Masters Thesis)
caption: 초해상화 단계에 깊이 정보를 활용해 저심도 영역을 제외한 전경(前景) 영역의 물체만 초해상화하는 딥러닝 기반의 방법을 제안합니다. 
description: >
  기존 초해상화 알고리즘을 인물 위주의 컨텐츠에서 실행하는 경우 굳이 개선할 필요가 없는 흐릿한 배경까지 구분 없이 초해상화해 성능이 저하되고 불필요한 리소스가 낭비되는 문제가 있습니다. 본 연구에서는 깊이 정보를 이용해 중요한 인물 영역만 추출한 후 초해상화 알고리즘을 적용하는 부분 초해상화 방법론을 제안합니다.
featured: true
date: 20 Dec 2020
image: 
  path: /assets/img/projects/SSU_partialSR_head.png
  srcset: 
    1920w: /assets/img/projects/SSU_partialSR_head.png
    960w:  /assets/img/projects/SSU_partialSR_head@0,5.png
    480w:  /assets/img/projects/SSU_partialSR_head@0,25.png
links:
  - title: 얕은 심도 콘텐츠를 위한 깊이 정보 기반 부분 초해상화 기법
    url: http://oasis.dcollection.net/srch/srchDetail/200000361093
accent_color: '#4fb1ba'
accent_image: 
  background: url('/assets/img/projects/partialSR_accentImg.png') center/cover
theme_color: '#193747'
sitemap: false
---

## 연구 개요 및 소개
### 연구 초록
최근 등장한 딥러닝 기반의 초해상화 연구는 괄목할 만한 성능 향상을 이루어 왔으나, 저심도 영상물에서 굳이 개선할 필요가 없는 흐릿한 배경까지 구분 없이 초해상화 하므로 오히려 성능이 하락하는 한계가 존재한다. 본 논문에서는 이러한 문제를 개선하기 위해 초해상화 단계에 깊이 정보를 가미함으로써 저심도 영역을 제외한 전경(前景) 영역의 물체만 초해상화하는 딥러닝 기반의 방법을 제안한다. 먼저 YouTube를 통해 수집된 고해상도 영상으로부터 고해상도 이미지들을 추출하고, 단안 깊이 추정 알고리즘을 활용해 얻어진 깊이 정보 이미지를 하나의 이미지에 담아 학습용 이미지 세트를 구축한다. 이 이미지들을 통해 학습 단계에서는 초해상화 모델로 하여금 저해상도 영상과 그에 대응되는 고해상도 영상의 관계를 학습하되, 깊이 정보 이미지로부터 전경에 해당하는 영역 내에서만 학습의 입력 피쳐가 추출될 수 있도록 한다. 테스트 단계에서는 저해상도 이미지를 원하는 목표 배수만큼 초해상화한 후 단안 깊이 추정 알고리즘으로부터 얻어진 깊이 정보 이미지를 이용해 배경에 해당하는 추론 결과를 무시하고 전경 영역만 남기도록 한다. 제안하는 방법을 통해 불필요한 저심도 배경 영역을 배제한 채 전경 영역에 초점을 맞추어 초해상화 함으로써 불필요한 성능 하락을 막을 수 있게 된다.


### 데이터셋 소개 
![dataset preview](/assets/img/projects/thumbnail_KpopStarsSR_Remastered.png)
KPopStarsSR Dataset은 Train Set 953장, Test Set 40장(2020년 12월 기준)으로 구성된 인물 위주 고해상도 데이터셋입니다. 3840*2160 해상도의 4K 이미지들로 구성되어 있으며, RGB, Monocular Depth Image, Masked RGB 이미지가 합쳐진 단일 RGB 이미지로 구성되어 있습니다. KPopStarsSR Dataset은 공식 K-Pop Music Video, Fancam 등의 매체로부터 추출되었으며, 선명한 전경 인물이 영역이 존재하는 이미지 위주로 구성되어 있습니다.



- 데이터셋 구축 방법: 
  ![dataset_preprocess](/assets/img/projects/partialSR_dataset_preprocessing.png)

### Loss Function & Depth Based Patch Sampling Methods
-  <b>Depth Semantic Loss(예정):</b> Monocular Depth Estimation 모델로부터 추출된 Depth Image를 기반으로 SR 대상 이미지의 전경과 배경을 구분하고, 심도에 따라 각각 다른 loss weight를 부여하도록 하는 loss function입니다.
  ![loss](/assets/img/projects/partialSR_loss.png)
- <b>Depth Based Patch Sampling(깊이 이미지 기반 학습 이미지 패치 샘플링 기법):</b> Depth Image를 통해 추출된 Foreground Mask로부터 학습용 이미지 패치를 추출하는 샘플링 알고리즘입니다. 아래 세 가지 알고리즘을 사용할 수 있으며, 육안상 품질은 Random Patch Sampling 방식이 제일 우수하고, PSNR/SSIM metric상의 성능은 Jitter Patch Sampling 방식이 가장 우수합니다.
  ![loss](/assets/img/projects/partialSR_002.png)
  - ⓐ Foreground Unique Patch Sampling: 전경 마스크 내부를 꽉 채우도록 패킹해 패치를 추출합니다.
  - ⓑ Foreground Random Patch Sampling: 전경 마스크 내부의 랜덤한 위치에서 패치를 추출합니다.
  - ⓒ Foreground Jitter Patch Sampling: Unique Patch Sampling과 Random Patch Sampling을 섞은 방법으로, Unique Patch Sampling에서 결정된 고유한 패치 내부에서 랜덤하게 패치의 위치를 추출합니다.
  
### Train & Test Phase
- <b>Train Phase:</b> Depth Based Patch Sampling 기법을 통해 전경 마스크 내에서만 High Resolution 이미지 패치를 추출하고, Bicubic Downsampling을 통해 1/4 사이즈로 축소한 후 Super Resolution 모델에 넣어 학습을 진행합니다.
  ![train](/assets/img/projects/partialSR_001_1.png)
- <b>Test Phase: </b> 3채널 Low Resolution 이미지를 입력받아 Low Resolution Depth Mask와 Super Resolved RGB Image를 생성한 후, Post Processing 과정에서 두 이미지를 활용해 전경만 초해상화된 이미지를 합성합니다.
  ![test](/assets/img/projects/partialSR_001_2.png)

### Performance
![sample_img](/assets/img/projects/partialSR_sample.png)
![performance](/assets/img/projects/partialSR_performance.png)


### 참여부문 소개 
- <b>참여 기간:</b> 2020.07.01 ~ 2020.09.30
- <b>참여 연구원 수:</b> 1명
- <b>참여 내용</b>
  - Python Multiprocess를 활용한 데이터셋 구축 
  - 전체 훈련 파이프라인 및 Loss 설계